<script setup lang="ts">
// setup语法糖，相当于export default defineComponent({setup() {})；setup（）生命周期函数里面写;
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
import MyChild from './components/MyChild.vue';
import { ref, reactive, onMounted, computed, watch,watchEffect, provide} from 'vue';
import Mitt from './mitt/mitt';
import { useRoute, useRouter, onBeforeRouteLeave, onBeforeRouteUpdate } from 'vue-router'



// ref创建响应式，类似data（）里面return的数据
let msg = ref(0);
// 通过value获取
msg.value = 1;
function changeMsg() {
  msg.value++;
}
// 类似ref，用于创建响应式对象；ref用于简单数据类型，ref要加.value取值
let obj = reactive({
    name:'juejin',
    age:3
})
const changeObj = () => {
    obj.name = 'hello juejin'
    obj.age = 4
}

const year = ref(2022)
// 计算属性
let nowYear = computed(() => {
  return year.value + obj.age
})
// 监听
watch(nowYear, (newValue, oldValue) => {
  console.log(nowYear.value)
  console.log(`year从${oldValue}变成了${newValue}`)
})

// 不需要指定属性，只要依赖的变量有改变就触发
watchEffect(()=>{
  console.log('watchEffect')
  console.log(nowYear.value)
})


// onMounted生命周期函数
onMounted(()=>{
  console.log('挂载完成')
})

function getMsgFromChild(msg: string) {
   console.log(msg)
}

const provide1 = ref('多层传递msg1')
provide("provide1",provide1)

// 事件总线
Mitt.on('sendMsg', (msg)=> {
  console.log(msg)
  console.log('接受到了消息')
})


const router = useRouter()
const route = useRoute()
//获取params 注意是route
route.params
//获取query
route.query

//路由守卫
onBeforeRouteUpdate((to, from, next)=>{//当前组件路由改变后，进行触发
    next() 
})
onBeforeRouteLeave((to, from, next)=>{//离开当前的组件，触发
    next() 
})
function goRouter(path: string) {
  router.push(path)

}

</script>

<template>
  <header>
    <!-- 数据响应式 -->
    <div @click="changeMsg">click me {{ msg }}</div>
    <div @click="changeObj"> click me {{obj.name}}</div>
    <div class="wrapper">
      <MyChild msg="父组件msg" @send-msg="getMsgFromChild" @otherFun="getMsgFromChild" msg2="穿透2"></MyChild>
      <div>路由跳转</div>
      <div @click="goRouter('home')">home</div>
      <div @click="goRouter('about')">about</div>
      <nav>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
