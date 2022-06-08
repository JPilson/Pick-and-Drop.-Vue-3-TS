<template>
  <div>
    Moving from {{movingFromDiv}} To {{movedTo}}
  </div>
  <div class="container"  v-for="(block,blockIndex) in blocks" :key="blockIndex"  @dragover.prevent = "" @dragenter="onDragEnter($event)" @dragleave="onDragLeave($event)" @drop.prevent="onDrop($event,blockIndex)">
      <div class="container-header"> Block {{blockIndex}}</div>

    <div class="items">
      <ItemCompo class="container-item" :item="item" v-for="(item,index) in block.items" :key="index" :draggable="true"  @dragstart="onDragStart($event,index,item)" @dragend="onDragEnd($event,index)"/>

    </div>
    <DropZone style="visibility: hidden" />
  </div>

  <div class="container" style="margin-top: 50px">
    <div class="items">
      <ItemCompo class="container-item" :item="item" v-for="(item,index) in items" :key="index" :draggable="true" @dragstart="onDragStart($event,index,item,'parent')"/>
    </div>
  </div>

</template>

<script lang="ts">
import {reactive, ref} from "vue";
import DropZone from "./DropZone.vue";
import type {Block, BlockItem} from "./Helper";
import ItemCompo from "./ItemCompo.vue";

interface DragItem {
  item:BlockItem,
  from:any,
  element:HTMLElement,
  collection:"instance"|"parent"
}
export default {
  name: "TierList",
  components:{
    ItemCompo,
    DropZone
  },
  setup(){
    const items:Array<BlockItem> = [
      {name:"Instagram",id:"insta",icon:"logo-instagram.svg"},
      {name:"WhatsApp",id:"whatsapp",icon:"bxl-whatsapp.svg"},
      {name:"Facebook",id:"face",icon:"bxl-facebook-circle.svg"},
      {name:"Snapchat",id:"face",icon:"bxl-snapchat.svg"},
      {name:"Android",id:"android",icon:"logo-android.svg"},
      {name:"Angular",id:"angular",icon:"logo-angular.svg"},
      {name:"Apple",id:"apple",icon:"logo-apple.svg"},
      {name:"Microsoft",id:"apple",icon:"logo-microsoft.svg"},
      {name:"React",id:"react",icon:"logo-react.svg"},
      {name:"Vue",id:"vue",icon:"logo-vue.svg"},

    ]
    const blocks = reactive<Array<Block>>([{items:[]},{items:[]}])
    const dragged = ref<DragItem|null>(null)
    const movingFromDiv = ref<any>(null);
    const movedTo = ref<any>(null);

    function onDragStart(e:any,index:number|string,draggedItem:any,collection:"instance"|"parent" = "instance") {
        e.dataTransfer.setData("text/plain", `${index}`);
        e.target.style.opacity = 0.5;
        dragged.value = {
          item:draggedItem as DragItem,
          from:index,
          collection:collection,
          element: e.target
        } // keeping the dragged item

    }

    function  onDragEnd (e:any,index:number){
      e.target.style.opacity = 1
      e.target.querySelector('.item-placeholder').visibility = "hidden"
      
    }

    function  onDrop(e:any,index:number, ){
        if(!blocks[index].items.includes(dragged.value.item))
            blocks[index].items.push(dragged.value.item);

        //Delete item from array if it was not moved from the parent
        if(dragged.value.collection!=='parent' && blocks[dragged.value.from].items )
          blocks[dragged.value.from].items.filter(item => item.id !== dragged.value.item.id)

      if(dragged.value.collection=='parent')
        dragged.value.element.style.opacity = 1

        e.target.querySelector('.item-placeholder').visibility = "hidden"

    }
    function onDragEnter(e:any){
      e.target.querySelector(".item-placeholder").style.visibility = "visible"
      e.target.querySelector(".item-placeholder").style.position = "relative"
    }

    function onDragLeave(e:any){
      e.target.querySelector(".item-placeholder").style.visibility = "hidden"
      e.target.querySelector(".item-placeholder").style.position = "absolute"
    }

    return {items,onDragStart,onDragEnd,onDrop,onDragEnter,onDragLeave
    ,movingFromDiv,
      movedTo,blocks}
  }
}
</script>

<style scoped lang="scss">
    $border-color: #efefef;
    $item-bg : #93f1c6;
    $item-text : #1a8053;
  .container {
    display: flex;
    border: 2px solid $border-color;
    border-radius: 8px;
    margin: 2px;

    .container-header{
      background: $border-color;
      font-weight: bold;
      text-transform: uppercase;
      padding: 20px;
    }

    .items {
      display: flex;
      justify-content: space-around;
      overflow: auto;
      justify-items: stretch;
      margin: 4px;

      .container-item {
        padding: 18px;
        border-radius: 8px;
        font-weight: bolder;
        font-size: 12px;
        color:  $item-text;
        text-transform: uppercase;
        background: $item-bg;

        margin: 4px;
      }
      .container-item:hover {
        cursor: move;
      }

    }

  }

</style>