<template>
  <div >
    {{ item.name }}
  </div>
</template>

<script lang="ts">
import  {BlockItem} from "./Helper";
import {PropType} from "vue";
export default {
  name: "ItemCompo",
  props: {
    item: {
      type: Object as PropType<BlockItem>,
      required: true
    }
  },
  setup(){

  }
}
</script>

<style scoped>

</style>