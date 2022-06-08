<template>
  <div class="item-placeholder">+</div>
</template>

<script lang="ts">
export default {
  name: "DropZone",
  setup(){

  }
}
</script>

<style lang="scss">
$border-color: #efefef;
.item-placeholder {

  width: 60px;
  height: auto;

  margin: 8px;
  border-radius: 8px;
  display: grid;
  align-content: center;
  font-weight: bolder;
  font-size: 18px;
  color: $border-color;
  border:  2px  solid $border-color  ;
  border-style: dashed;

}
</style>