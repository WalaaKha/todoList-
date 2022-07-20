<template>
  <div class="item">
    <input ref="completedInput"
      :completed="completed"
      :completedFromProps="item.completed"
      type="checkbox"
      v-model="completed"
      true-value="1"
      false-value="0"
      @change="updateCheck(completed)"
    />
    <span :class="[item.completed ? 'completed' : '', 'item-text']">{{
      item.name
    }}</span>
    <button @click="removeItem()" class="trashcan">-</button>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import axios from "axios";
//import todoItem from "@/types/todoItem";

export default defineComponent({
  props: {
    item: {
      required: true,
      type: Object,
    },
  },
  emits: ["itemChanged", "statusChanged"],
  setup(props, ctx) {
    let completed = ref<boolean>(props.item.completed);
    console.log("completed = ");
    console.log(completed.value);
    //const emit = defineEmits(["itemChanged"]);
    //const completed = ref<number>(props.item.completed);
    const updateCheck = (completed: boolean) => {
      //completed = props.item.completed;
      ctx.emit("statusChanged", completed);
      console.log("updateCheck: " + props.item);
      console.log(props.item);
      axios
        .put("http://127.0.0.1:8000/api/item/" + props.item.id, {
          item: props.item,
        })
        .then((response) => {
          if (response.status == 200) {
            ctx.emit("itemChanged");
            //this.$emit("itemChanged");
          }
        })
        .catch((error) => {
          console.log("error in updating item: " + error);
        });
    };
    const removeItem = () => {
      //completed = props.item.completed;
      axios
        .delete("http://127.0.0.1:8000/api/item/" + props.item.id)
        .then((response) => {
          if (response.status == 200) {
            ctx.emit("itemChanged");
            //this.$emit("itemChanged");
          }
        })
        .catch((error) => {
          console.log("error in deleting item: " + error);
        });
    };
    return {
      updateCheck,
      removeItem,
      completed,
    };
  },
  mounted(){
    console.log("using mounted");
    console.log(this.$refs.completedInput);
  },
  updated(){
    console.log("using updated");
    console.log(this.$refs.completedInput);
  }
  /*props: ["item"],
  methods: {
    updateCheck() {
      axios
        .put("http://127.0.0.1:8000/api/item/" + item.value.id, {
          item: this.item,
        })
        .then((response) => {
          if (response.status == 200) {
            this.$emit("itemChanged");
          }
        })
        .catch((error) => {
          console.log("error in updating item: " + error);
        });
    },
    removeItem() {
      axios
        .delete("http://127.0.0.1:8000/api/item/" + this.item.id)
        .then((response) => {
          if (response.status == 200) {
            this.$emit("itemChanged");
          }
        })
        .catch((error) => {
          console.log("error in deleting item: " + error);
        });
    },
  },*/
});
</script>
<style scoped>
.completed {
  text-decoration: line-through;
  color: #999999;
}
.item-text {
  width: 100%;
  margin-left: 20px;
}
.item {
  /*display: flex;*/
  justify-content: center;
  align-content: center;
}
.trashcan {
  background: #e6e6e6;
  border: none;
  color: #ff0000;
  outline: none;
  float: right;
}
</style>