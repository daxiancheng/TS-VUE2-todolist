<template>
  <div class="home">
    <input type="text" v-model="listContent" class="inputClass" />
    <button @click="addList" class="buttonClass">添加</button>
    <hello-world :content="listItem" />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import { itemType } from "../interface/index";
import HelloWorld from "@/components/HelloWorld.vue"; // @ is an alias to /src

@Component({
  components: {
    HelloWorld,
  },
})
export default class Home extends Vue {
  private listContent: string = "";
  private listItem: Partial<itemType> = {};
  addList(): void {
    if (this.listContent) {
      this.listItem = { content: this.listContent, status: "todo" };
      this.listContent = "";
    } else {
      alert("请先填写任务!");
    }
  }
}
</script>

<style lang="scss" scoped>
.inputClass {
  outline: none;
  background: none;
  border: 1px solid gray;
  margin-right: 10px;
  border-radius: 2px;
  &:focus {
    outline: none;
  }
}
.buttonClass {
  border: 1px solid gray;
  background: #fff;
  outline: none;
  border-radius: 2px;
}
</style>