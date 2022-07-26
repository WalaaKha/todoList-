<template>
  <div class="addItem">
    <input type="text" v-model="item.name" />
    <button
      :class="[item.name ? 'active' : 'inactive', 'plus']"
      @click="addItem()"
    >
      +
    </button>
  </div>
</template>

<script lang="ts" allowJs="true">
import { defineComponent, ref } from "vue";
import axios from "axios";
import todoItem from "@/types/todoItem";

export default defineComponent({
  emits: ['reloadlist'],
  setup(props, ctx) {
    const item = ref({
      //id: 0,
      name:'',
      completed: 0
    });
    const addItem = () => {
      //console.log("start adding ...");
       if (item.value.name == '') {
        return;
      }
      axios
        .post("http://127.0.0.1:8000/api/item/store", {
          item: item.value,
        })
        .then((response) => {
          if (response.status == 201) {
            item.value.name = "";
             ctx.emit("reloadlist");
          }
        })
        .catch((error) => {
          console.log("error in adding item: " + error);
        });
    };
    return {
      item,
      addItem,
    };
  },

  /*data: function () {
    return {
      item: {
        name: "",
      },
    };
  },
  methods: {
    addItem() {
      console.log("start adding ...");
      if (this.item.name == "") {
        return;
      }
      axios
        .post("http://127.0.0.1:8000/api/item/store", {
          item: this.item,
        })
        .then((response) => {
          if (response.status == 201) {
            this.item.name = "";
          }
        })
        .catch((error) => {
          console.log("error in adding item: " + error);
        });
    },
  },*/
});
</script>
<style scoped>
.addItem {
  display: flex;
  /*justify-content: center;
  align-items: center;*/
}
input {
  background: #f7f7f7;
  outline: none;
  padding: 5px;
  margin-right: 10px;
  width: 40%;
}
.plus {
  font-size: 20px;
}
.active {
  color: #00ce25;
}
.inactive {
  color: #999999;
}
</style>
