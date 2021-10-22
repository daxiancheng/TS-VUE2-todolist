<template>
  <div class="hello">
    <ul>
      <li v-for="(item, index) in dataList" :key="item.value" @mouseenter="mouseSelect(item)" @mouseleave="leaveSelect">
        <input
          type="checkbox"
          :checked="item.status === 'done'"
          @change="checkSelet($event, item)"
        />
        <span>{{ item.name }}: {{ item.content }}</span>
        <img src="../assets/delete.png" width="16px" class="imgClass" v-show="delIconIndex === index" @click="delItem(index)">
      </li>
    </ul>
  </div>
</template>
<script lang="ts">
import { Component, Prop, Vue, Watch } from "vue-property-decorator";
import { itemType } from "../interface/index";

@Component
export default class HelloWorld extends Vue {
  // 感叹号是非null和非undefined的类型断言
  @Prop() content!: Partial<itemType>;
  @Watch("content", { deep: true }) addContent(newValue: Partial<itemType>) {
    let length = this.dataList.length;
    let item = { name: `任务${length + 1}`, ...newValue };
    this.dataList.push(<itemType>item);
    localStorage.setItem("dataList", JSON.stringify(this.dataList));
  }
  private dataList: Array<itemType> = localStorage.getItem("dataList")
    ? JSON.parse(<string>localStorage.getItem("dataList"))
    : [];
  private delIconIndex:number = -1
  mounted() {
    this.setValue();
  }
  setValue() {}
  checkSelet(e: Event, item: itemType) {
    let checked = (<HTMLInputElement>e.target).checked;
    item.status = checked ? "done" : "todo";
    localStorage.setItem("dataList", JSON.stringify(this.dataList));
  }
  mouseSelect(item: itemType) {
    this.delIconIndex = this.dataList.indexOf(item)
  }
  leaveSelect() {
    this.delIconIndex = -1
  }
  delItem(index: number){
    this.dataList.splice(index, 1)
    localStorage.setItem("dataList", JSON.stringify(this.dataList));
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
ul {
  list-style-type: none;
  display: inline-block;
  padding: 20px 0px;
  width: 226px;
  border: 1px solid gray;
}
li {
  list-style-type: none;
  border-bottom: 1px solid #ddd;
  border-top: 1px solid #ddd;
  padding: 4px 0px;
  margin-bottom: 6px;
  text-align: left;
  overflow: hidden;
  &:hover{
    background: #bbb;
  }
  .imgClass{
    float: right;
    margin-right: 4px;
    margin-top: 2px;
  }
}
a {
  color: #42b983;
}
</style>
