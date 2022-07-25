<template>
  <div class="card">
    <!--begin::Header-->
    <div class="card-header border-0 pt-5">
      <h3 class="card-title align-items-start flex-column">
        <span class="card-label fw-bolder text-dark">Todo List </span>
      </h3>
    </div>
    <div class="card-body pt-5">
      <AddItemForm v-on:reloadlist="getList()" />
      <div>list view</div>
      <ListView :items="items" v-on:reloadlist="getList()" />
    </div>
  </div>
</template>
<script lang="ts">
import { defineComponent, onMounted, ref } from "vue";
import axios from "axios";
import AddItemForm from "@/components/testing/AddItemForm.vue";
import ListView from "@/components/testing/ListView.vue";
import todoItem from "@/types/todoItem";
export default defineComponent({
  name: "todolist",
  components: {
    AddItemForm,
    ListView,
  },
  setup() {
    const items = ref<todoItem[]>([]);
    const getList = () => {
      //console.log("get list starting...");
      axios
        .get("http://127.0.0.1:8000/api/items")
        .then((response) => {
          console.log("first");
          console.log(response.data);
          items.value = response.data;
          console.log("second");
          console.log(items.value);
        })
        .catch((error) => {
          console.log("Error in getting data" + error);
        });
    };
    console.log("third");
    console.log(items.value);
    onMounted(() => {
      getList();
    });

    console.log("forth");
    console.log(items.value);
    return {
      items,
      getList,
    };
  },
});
</script>
