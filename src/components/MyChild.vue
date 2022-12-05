<script setup lang="ts">
import { toRef, useAttrs, inject } from "vue";
import Mitt from '../mitt/mitt';

let props = defineProps<{
  msg: string
}>()

/*、

<script setup lang="ts">
// 定义一个接口来指定类型
interface Props {
  name: string
  count?: number
}

// 对 defineProps() 的响应性解构
// 默认值会被编译为等价的运行时选项
const { name, count = 100 } = defineProps<Props>()
</script>

*/

// 事件传递s
const emits = defineEmits(['sendMsg'])
const sendFun = () => {
  // emit一个方法给父组件
    emits('sendMsg', '我是子组件数据')
}
/** 
 * 
 * const emit = defineEmits({
  // 没有校验
  click: null,

  // 校验 submit 事件
  submit: ({ email, password }) => {
    if (email && password) {
      return true
    } else {
      console.warn('Invalid submit event payload!')
      return false
    }
  }
})

function submitForm(email, password) {
  emit('submit', { email, password })
}
*/


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
