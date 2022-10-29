<script setup lang="ts">
import { toRef, useAttrs, inject } from "vue";
import Mitt from '../mitt/mitt';

let props = defineProps<{
  msg: string
}>()

const emits = defineEmits(['sendMsg'])
const sendFun = () => {
  // emit一个方法给父组件
    emits('sendMsg', '我是子组件数据')
}

// 将props中的msg转换成响应式的
let parentMsg = toRef(props, 'msg')


const attrs = useAttrs()
//获取父组件方法和事件
console.log(attrs.msg) //Proxy {"msg2": "子组件msg"}
const getParentFun = () => {
    //调用父组件方法
    attrs.onOtherFun('穿透传递')
    console.log(attrs.msg2)
}
let inject1: any = inject('provide1');
console.log(inject1.value)

// 事件总线
Mitt.emit('sendMsg', '发送消息')
</script>

<template>
  <div class="greetings">
    <div>我的参数： {{ msg }}</div>
    <div @click="sendFun">click me 发送数据给父组件</div>
    <div @click="getParentFun">click me 调用穿透传递的函数</div>
  </div>
</template>

<style scoped>
</style>
