<template>
  <div>
    <div v-for="(item, index) in items" :key="index">
      <list-item
        :item="item"
        v-on:statusChanged="(completed) => item.completed = !item.completed"
        v-on:itemChanged="$emit('reloadlist')"
        class="item"
      />
    </div>
  </div>
</template>

<script lang="ts">
import ListItem from "@/components/testing/ListItem.vue";
export default {
  props: ["items"],
  components: {
    ListItem,
  },
 /* methods: {
    itemChangedFun(){
      this.$emit('reloadlist')
    }
  }*/
};
</script>
<style scoped>
.item {
  background: #e6e6e6;
  padding: 5px;
  margin: 5px;
}
</style>