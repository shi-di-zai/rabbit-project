# vue-rabbit

This template should help get you started developing with Vue 3 in Vite.

## Recommended IDE Setup

[VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur) + [TypeScript Vue Plugin (Volar)](https://marketplace.visualstudio.com/items?itemName=Vue.vscode-typescript-vue-plugin).

## Customize configuration

See [Vite Configuration Reference](https://vitejs.dev/config/).

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```

### Lint with [ESLint](https://eslint.org/)

```sh
npm run lint
```
npm init vue@latest
npm install
npm run dev
p21
项目初始化和git管理
git init
git add .
git commit -m "first commit"
p22
别名路径联想设置
创建jsconfig.json文件，内容如下：   
{
    "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  },
}
在vite.config.ts文件中配置别名路径：
import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'
并且在pinia插件中配置：
AutoImport({
      resolvers: [ElementPlusResolver()],
    }),
p23elementPlus 按需引入
安装elementPlus：
npm install element-plus --save
然后安装unplugin-vue-components 和 unplugin-auto-import：
npm install -D unplugin-vue-components unplugin-auto-impo rt
p24
elementplus 主题色定制：
安装sass：
npm install -D sass
准备定制样式文件styles/element/index.scss：
在vite.config.js中配置scss预处理器
Components({
      // 1.配置 elementPlus 采用 sass 样式配色系统
      resolvers: [ElementPlusResolver({ importStyle: "sass" })],
    }),
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
  },
  css: {
    preprocessorOptions: {
      scss: {
        // 2.自动导入定制化样式文件进行样式覆盖
        additionalData: `
          @use "@/styles/element/index.scss" as *;
          @use "@/styles/var.scss" as *;
        `,
      }
    }
  }
p25
axios基础配置
安装axios：
npm install axios --save
找到utils文件夹，创建http.js文件，内容如下：
axios基础封装：
import axios from 'axios'
const instance = axios.create({
  baseURL: 'http://localhost:3000',
  timeout: 10000
})
添加请求拦截器：
instance.interceptors.request.use(config => {
  // 在发送请求之前做一些处理
  return config
}, error => {
  // 对请求错误做些什么
  return Promise.reject(error)
})
添加响应拦截器：
instance.interceptors.response.use(response => {
  // 对响应数据做点什么
  return response
}, error => {
  // 对响应错误做点什么
  return Promise.reject(error)
})
导出axios实例：
export default instance
在apis中创建测试接口testapi.js，内容如下：
import http from '@/utils/http'
export const testApi = () => {
  return http.get('/test')
}
最后在main.js中引入并使用测试接口：
import { getCategory} from '@/apis/category'
// 调用测试接口
getCategory().then(res => {
  console.log(res)
}).catch(err => {
  console.log(err)// 错误处理
})
p26
路由配置设计
一级路由：整体页面切换
路径patH:#/      路径path：#/login
在views里面创建俩个文件：layout和login
然后在router文件里面创建一个index.js文件，内容如下：
import { createRouter, createWebHashHistory } from 'vue-router'// 引入路由包，创建router实例对象，创建hash模式的路由
import Layout from '../views/layout/Layout.vue'
import Login from '../views/login/Login.vue'
const routes = [
  {
    path: '/',
    name: 'Layout',
    component: Layout,
    children: [
      {
        path: 'login',
        name: 'Login',
        component: Login
      }
    ]
  }
]
const router = createRouter({
  history: createWebHashHistory(),
  routes
})
export default router
在app.vue文件中引入router：
<template>
  <router-view />
</template>
二级路由：页面内切换
路径path：#/       路径path：#/category
在views里面创建俩个文件：category和home
然后在router文件里面index.js文件添加一个二级路由，内容如下：
导入俩个组件：
import home from '../views/home/Home.vue'
import category from '../views/category/Category.vue'
然后在routes数组中添加一个二级路由：
{
  path: '/category',
  name: 'Category',
  component: category,
  children: [
    {
      path: '',
      name: 'Home',
      component: home
    }
  ]
}
在layout里面加二级路由的出口
<template>
  <div class="layout">
    <router-view />
  </div>
</template>
三级路由：页面内的功能切换
四级路由：页面内的功能内的细节切换
五级路由：页面内的功能内的细节内的小功能切换
六级路由：页面内的功能内的细节内的小功能内的细节切换
1.路由设计的依据是什么？
内容的切换方式
2.默认二级路由如何设置？
path配置项置空
27.静态资源引入和ErrorLen安装
图片资源和样式资源
资源说明：
1.实际工作中的图片一般由ui设计师提供，常见的图片格式有png，svg，jpg，gif等都是ui切图给前端
2.样式资源一般由前端工程师编写，常见的样式文件格式有css，less，sass，stylus等
3.字体资源一般由前端工程师编写，常见的字体文件格式有ttf，woff，woff2等
4.第三方库资源一般由前端工程师引入，常见的第三方库有jquery，bootstrap，vue，element-plus等
5.错误日志一般由前端工程师记录，常见的错误日志格式有json，txt，log等
6.静态资源一般由前端工程师托管，常见的静态资源托管平台有阿里云oss，腾讯云cos，github等
资源操作：
1.图片资源-把image文件放在src/assets/images目录下
2.样式资源-把common.scss文件放在src/styles目录下
在main.js文件中引入common.scss文件
import './styles/common.scss'
3.字体资源-把font文件放在src/assets/fonts目录下
4.第三方库资源-把第三方库文件放在src/lib目录下
5.错误日志-把错误日志文件放在src/logs目录下
安装插件ErrorLen
p28
scss文件的自动导入
原因：在项目里面一些组件共享的色值会以scss变量的方式统一放到一个名为var.scss的文件中，正常组件中使用，需要先导入scss文件，再使用内部的变量，比较繁琐，因此可以用插件自动导入scss文件直接使用内部变量
新增一个var.scss文件，存入色值变量。内容如下：
$xtxColor: #27ba9b;
$helpColor: #e26237;
$sucColor: #1dc779;
$warnColor: #ffb302;
$priceColor: #cf4444;
通过vite.config.ts文件配置scss预处理器，并配置自动导入var.scss文件
@import './styles/var.scss';
然后在需要使用变量的组件中直接使用即可，不需要再导入scss文件，直接使用变量
例如：
<div class="test">
 test
 </div>
 <style lang="scss">
.test {
   color: $xtxColor;
 }
 </style>
p29
layout模块静态模版搭建
在views/layout/components目录下创建LayoutHeader.vue,;layoutFooter.vue,layoutnav.vue三个组件
p30
Layout字体图标引入
如何引入？
阿里的字体图标库支持多种引入方式，此项目采用的是font-class方式，具体步骤如下：
1.下载字体图标库
2.将下载的字体文件放入项目的src/assets/fonts目录下
3.在main.js文件中引入字体文件
import './assets/fonts/iconfont.css';
4.在需要使用字体图标的地方，添加对应的class即可
<i class="iconfont icon-xxx"></i>
最后在index.html文件中引入阿里字体库的cdn链接
<link rel="stylesheet" href="//at.alicdn.com/t/font_2143783_iq6z4ey5vu.css">
p31
layout-一级导航渲染
实现步骤：
1.根据接口文档封装接口函数
2.发送请求获取数据列表
3.v-for渲染页面
在apis里面创建一个layout.js文件，内容如下：
import http from '@/utils/http'
export const getMenuList = () => {
  return http.get('/menu/list')
}
再调用接口
import {getCategoryAPI} from '@/apis/layout'
const getCategory = async () => {
  const res = await getCategoryAPI()
  console.log(res)
}
发起接口请求，获取数据列表
import {onMounted} from 'vue'
onMounted(() => {
  getCategory()
})
渲染模版测试响应式数据
const categoryList=ref([])
在console.log(res)前面添加categoryList.value=res.result
最后在<li class="home" v-for="item in categoryStore.categoryList" :key="item.id">
中添加v-for="item in categoryStore.categoryList" :key="item.id"
p32
layout-吸顶导航交互实现：浏览器在上下滚动的过程中，如果距离顶部的距离大于某个值，则吸顶导航固定在顶部，否则跟随页面滚动
在views/layout/components目录下创建layoutfiexd.vue文件，然后在index.vue文件中引入
import LayoutFixed from '../components/layoutFixed/LayoutFixed.vue'
然后渲染<layoutfiexd />
opacity
加入show类名
引入插件vueuse实现滚动距离
npm i @vueuse/core
在layoutFixed.vue文件中引入
// vueUse
import { useScroll } from '@vueuse/core'
import { useCategoryStore } from '@/stores/category'

const { y } = useScroll(window)
const categoryStore =useCategoryStore()
p33
layout-pinia优化重复请求
俩个导航中的列表是完全一致的，但是要发送俩次网络请求，存在浪费。通过pinia集中管理数据，再把数据给组件使用
1.在store目录下创建category.js文件，内容在文件里
2.在layoutfixed.vue文件中引入pinia
import { useCategoryStore } from '@/stores/category'
使用pinia中的数据
const categorystore = useCategoryStore()
直接在li标签上绑定数据
<li class="home" v-for="item in categorystore.categoryList" :key="item.id">
{{ item.name }}
p34
Home-整体结构拆分与分类实现
在views/home目录下创建各类home组件，然后在index.vue文件中引入并且渲染
p35
Home-baneer轮播图实现
准备模板-》熟悉elementplus相关组件-》获取接口数据-》渲染组件
直接在elementplus官网上找到轮播图组件，然后在home组件中引入
<el-carousel>
  <el-carousel-item v-for="(item, index) in bannerList" :key="index">
    <img :src="item.img" alt="">
  </el-carousel-item>
</el-carousel>
p36
Home-面板组件封装
组件封装解决了什么问题？
1.代码复用：相同的功能模块可以抽取成一个组件，方便复用
2.可维护性：修改某一处代码，只需要修改组件，不用修改其他地方
例如新鲜好物和人气推荐模块，在结构上非常相似，只是内容不同，通过组件封装可以实现复用结构的效果
步骤
1.创建HomePanel.vue组件准备静态模板
2.抽象可变部分
-主标题和副标题是纯文本，可以抽象成props传入
-主体内容是复杂的模板，抽象成插槽传入
//定义props
props: {
  title: {
    type: String,
    default: '新鲜好物'
  },
  subTitle: {
    type: String,
    default: '每日必抢'
  }
}
<slot />->主体内容插槽
p37
Home-新鲜好物业务实现
准备模板-》定制props-》定制插槽内容（接口和渲染模板）
在Homenew.vue文件中引入HomePanel组件，并传入props
<homepanel :title="title" :subTitle="subTitle">
导入findNewAPI接口
import { findNewAPI } from '@/apis/home'
// 定义数据
const newGoods = ref([])
// 发送请求
const getNewGoods = async () => {
  const res = await findNewAPI()
  newGoods.value = res.result
}
// 渲染模板
 <ul class="goods-list">
            <li v-for="item in newList" :key="item.id">
                <RouterLink :to="`/detail/${item.id}`">
                    <img v-img-lazy="item.picture" alt="" />
                    <p class="name">{{ item.name }}</p>
                    <p class="price">&yen;{{ item.price }}</p>
                </RouterLink>
            </li>
        </ul>
p38
Home-图片懒加载实现
场景：当页面滚动到某个位置时，图片才开始加载，提高用户体验
1.空指令实现
app.directive("img-lazy",{
  mounted(el,binding)
  {
    //el:指令绑定的那个元素 img
    //binding:指令的绑定值，即图片的路径
  }
})
指令用法：<img v-img-lazy="item.picture" >
2.指令逻辑实现
useIntersectionObserver(
  el,
  ([{isIntersecting}])=>{
    console.log(isIntersecting)
    if(isIntersecting){
    //进入视口区域
    el.src=binding.value
  }
  },
)
在main.js中引入
// 全局指令注册
import { lazyPlugin } from '@/directives/index'
app.use(lazyPlugin)
在directives目录下创建index.js文件，配置全局指令
p39
Home-懒加载指令优化
懒加载指令的逻辑直接写到入口文件中，合理吗？
不合理，入口文件通常只做一些初始化的事情，不应该包含太多的逻辑代码，可以通过插件的方法把懒加载指令封装为插件，然后在入口文件中引入并使用。
const directivesPlugin={
  install(app){
    app.directive("img-lazy",{
      mounted(el,binding)
      {
        useIntersectionObserver(
          el,
          ([{isIntersecting}])=>{
            if(isIntersecting){
            //进入视口区域
            el.src=binding.value
          }
          },
        )
      }
}
然后在main.js中引入
import { directivesPlugin } from '@/directives/index'
app.use(directivesPlugin)
解决重复监听问题
useIntersectionObserve对于元素监听是一致存在的，除非手动停止监听，存在内存浪费，加入stop方法，在组件销毁时停止监听
p40
Home-product产品列表实现
创建HomeProduct.vue组件，准备静态模板
封装接口
import { getProductAPI } from '@/apis/home'
// 定义数据
const productList = ref([])
// 发送请求
const getProduct = async () => {
  const res = await getProductAPI()
  productList.value = res.result
}
渲染模板
<div class="product-list">
  <div class="product-item" v-for="item in productList" :key="item.id">
    <RouterLink :to="`/product/${item.id}`">
      <img v-img-lazy="item.picture" alt="" />
      <p class="name">{{ item.name }}</p>
      <p class="price">&yen;{{ item.price }}</p>
    </RouterLink>
  </div>
</div>  
P41
Home-Goodsitem组件封装
在很多个业务模块中都需要用到同样的商品展示模块，没必要重复定义，封装起来，方便复用
核心思想：把要显示的数据对象设计为props参数，传入什么数据对象就显示什么数据
创建GoodsItem.vue组件，准备静态模板
props: {
  item: {
    type: Object,
    default: () => ({})// 默认空对象
    }
  }内容在GoodsItem.vue文件中
  在HomeProduct.vue文件中引入GoodsItem组件，并传入props
  <goodsitem :item="item" />
p42
一级分类-整体认识和路由配置
在router目录下创建index.js文件，内容如下：
加上一个站位的id参数
在组件里面找到<Routerlink>标签，并添加id参数
<Routerlink ：to="/category/${item.id}">
p43
一级分类-面包屑导航实现
 <!-- 面包屑 -->
         <div class="bread-container">
            <el-breadcrumb separator=">">
               <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
               <el-breadcrumb-item>{{ categoryData.name }}</el-breadcrumb-item>
            </el-breadcrumb>
         </div>
准备组件模板-》封装接口函数-》调用接口获取数据-》渲染模板
封装函数在apis/category.js文件中
import http from '@/utils/http'
function getCategoryAPI(id) {
  return  request({
    url: `/category',
    params: {
      id
    }
  })
  }
获取数据在category/index.vue文件中
import { getCategoryAPI } from '@/apis/category'
const categoryData = ref({})
const getCategory = async () => {
  const res = await getCategoryAPI(id)
  categoryData.value = res.result
}
P44
一级分类-轮播图实现
分类轮播图实现：分类轮播图和首页轮播图的区别只有一个，接口参数不同，其余逻辑一致
改造分类轮播图的接口=》迁移首页轮播图逻辑
在接口文件里面找到home.js文件添加params参数
定义传参对象
const {distributionSite='1'}=params
设置return httpInstance({
  url: '/home/banner',
  params: {
    distributionSite// 新增参数
  }
})
在index.vue文件中引入home.js文件，并调用接口
import { getHomeAPI } from '@/apis/home'
const bannerList = ref([])
cinst getBanner = async () => {
  const res = await getHomeAPI({
    distributionSite: '2' // 新增参数
  })
  console.log(res) 
  bannerList.value = res.result.banners
}
渲染模板
p45
一级分类-激活状态控制和分类列表渲染
Routerlink组件已经默认支持激活样式显示的类名，只需给active-class属性设置对应的类名即可
在layout/components/layoutnav.vue文件中引入Routerlink组件，并设置active-class属性
<Routerlink active-class="active" :to="'/category/${iten.id}'">
{{ item.name }}
</Routerlink>
.active{
  color:$xtxColor;
  border-bottom:1px solid $xtxColor;
}
分类列表渲染
<!-- 分类数据 -->
         <div class="sub-list">
            <h3>全部分类</h3>
            <ul>
               <li v-for="i in categoryData.children" :key="i.id">
                  <RouterLink :to="`/category/sub/${i.id}`">
                     <img :src="i.picture" />
                     <p>{{ i.name }}</p>
                  </RouterLink>
               </li>
            </ul>
         </div>
         <div class="ref-goods" v-for="item in categoryData.children" :key="item.id">
            <div class="head">
               <h3>- {{ item.name }}-</h3>
            </div>
            <div class="body">
               <GoodsItem v-for="good in item.goods" :good="good" :key="good.id" />
            </div>
         </div>
p46
一级分类-解决路由缓存问题
什么是路由缓存?
响应路由参数的变化
使用带有参数的路由时需要注意的是，当用户从/users/johnny导航到/users/jolyne时，相同的组件实例将被重复使用。因为俩个路由都渲染同个组件，比起销毁再创建，复用则显得更加高效。不过，这也意味着组件的生命周期钩子不会被调用
解决路由缓存问题：
方案一：给router-view添加key属性
<router-view :key="$route.fullPath"></router-view>
方案二：使用beforeRouteUpdate导航钩子
beforeRouteUpdate钩子函数可以在每次路由更新之前执行，在回调中执行需要数据更新的业务逻辑即可
或者使用beforeRouteUpdate导航守卫
const User ={
  template: '<div>User</div>',
  async beforeRouteUpdate(to, from, next) {
    //路由做出响应
    this.userData=await fetchUser(to.params.id)
},
}
p47
一级分类-使用逻辑函数拆分业务
概念理解：基于逻辑函数拆分业务是指把同一个组件中独立的业务代码通过函数封装处理，提高代码可维护性
实现步骤：
1.按照业务声明以‘use'打头的逻辑函数
2.把独立的业务逻辑封装到各个函数内部
3.函数内部把组件中需要用到的数据或者方法return出去
4.在组件中调用函数把数据或者方法组合回来使用
调用业务逻辑函数
import { useBanner } from '@/views/Category/composables/useBanner'
import { useCategory } from '@/views/Category/composables/useCategory'
创建俩个use逻辑函数useBanner和useCategory
在index.vue文件中调用useBanner和useCategory函数，并把数据和方法组合回来使用
p48
二级分类-整体认识和路由配置
创建路由组件-》配置路由关系-》修改模板实现跳转
在views下创建一个SubCategory目录准备路由组件
配置路由关系
在路由文件中引入SubCategory组件并配置路由关系
添加{
  path: '/category/sub/:id',
  component：SubCategory,
}
最后修改模板实现跳转
在category的index.vue中修改模板，实现跳转
找到<Routerlink>标签，并添加id参数
<Routerlink ：to="/category/sub/${item.id}">
p49
二级分类-面包屑导航实现
封装接口-》调用接口渲染模板-》测试跳转
封装接口在apis/category.js文件中
import http from '@/utils/http'
export const getCategoryFilterAPI = (id) => {
  return  request({
    url: `/category/${id}/filter`,
    params: {
      id
    }
  })
}
在SubCategory.vue文件中调用接口并渲染模板
import {getCategoryFileterAPI} from '@/apis/category'
import {ref,onMounted} from 'vue'
import {useRoute} from 'vue-router'
import {GoodsItem} from '@views/Home/components/GoodsItem'
const route = userRoute()
const categoryData = ref({})
const categoryFilterData = asyns () => {
  const res= await getCategoryFileterAPI(route.params.id)
  categoryData.value=res.result
}
onMounted((=>{
  getCategoryData()
}))
渲染模板
渲染name和拼接路径<el-breadcrumb-item:to="{path:'/category/${categoryData.parent.id}'}">{{categoryData.parent.name}}
p50
二级分类-基础商品列表实现
封装接口-》准备基础参数-》获取数据渲染列表
封装接口在   apis/category.js文件中
export const getsubcategoryAPI = (data)=>{
  retrun http({
    url:'/category/goods/temporary',
    method:'POST',
    data  })
  }
  准备基础参数
@description: 获取导航数据
 * @data { 
     categoryId: 1005000 ,  // 分类id
     page: 1,// 页码
     pageSize: 20,// 每页条数
     sortField: 'publishTime' | 'orderNum' | 'evaluateNum'// 排序字段
   } 
 * @return {*}
 获取数据渲染列表
import { getsubcategoryAPI } from '@/apis/category'
import { ref, onMounted } from 'vue'
import { useRoute } from 'vue-router'
import { GoodsItem } from '@views/Home/components/GoodsItem'
const goodlist =ref([])
const reqData=ref({
  categoryID:route.params.id,
  page:1,
  pageSize:20,
  sortField:'publishTiem'
})
const getGoodList = async () =>{
  const res = await getsubcategoryAPI(reqData.value)
  console.log(res)
  goodList.value=res.result.items
}
onMounted(()=>{
  getGoodList()
})
渲染商品卡片
<div class="body">
<GoodsItem v-for="goods in goodList":goods="goods":key="goods.id"/>
</div>
p51 
二级分类-列表筛选功能实现
核心逻辑：点击tab,切换筛选条件参数sortField。重新发送列表请求
获取激活项数据-》使用新参数发送请求重新渲染列表
在<div class="sub-container">
            <el-tabs v-model="reqData.sortField" @tab-change="tabChange">
                <el-tab-pane label="最新商品" name="publishTime"></el-tab-pane>
                <el-tab-pane label="最高人气" name="orderNum"></el-tab-pane>
                <el-tab-pane label="评论最多" name="evaluateNum"></el-tab-pane>
            </el-tabs>
            <div class="body" v-infinite-scroll="load" :infinite-scroll-disabled="disabled">
绑定tabChange事件，切换sortField参数
tab切换回调
const tabchange = ()=>{
  console.log('tab切换了',reqData.sortField)
  reqData.value.sortField=1
  getGoodList()
}
p52
二级分类-列表无限加载实现
核心实现逻辑：使用elementplus提供的v-infinite-scroll指令监听是否满足触底条件，满足加载条件时让页面参数加一获取下一页数据，做老新数据拼接渲染
配置v-infinite-scroll指令-》页数加一获取下一页数据-》老数据和新数据拼接-加载完毕结束监听
配置v-infinite-scroll指令
<div class="body" v-infinite-scroll="load" :infinite-scroll-disabled="disabled">
配置页数加一获取下一页数据
const load=()=>{
  console.log('触底了，加载下一页数据')
  reqData.value.page+=1
  const res=await getSubcategoryAPI(reqData.value)
  goodList.value=[...goodList.value,...res.result.items]
  //加载完毕，停止监听
  if(res.result.items.length==0)
  {
    disabled.value=true
  }
  
}
}
p53
二级分类-定制路由滚动行为
定制路由行为解决什么问题？
在不同路由切换的时候，可以自动滚动到页面的顶部，而不是停留在原先的位置
解决方案：
vue-router支持scrollBehavior配置项，可以指定路由切换时的滚动位置
在router/index.js文件中配置scrollBehavior
  scrollBehavior(to, from, savedPosition) {
    return { top: 0 }
  },
}
p54
详情页-整体认识和路由配置
创建详情组件-》绑定路由关系（路由参数）-》绑定模板测试跳转
创建detail.vue组件准备静态模板
绑定路由关系
在router/index.js文件中配置路由关系
import Detail from '@views/Detail/index.vue'
加一个配置项
{
  path：'detail/:id',
   component:Detail
}
绑定模板测试跳转
在Home/components/GoodsItem.vue文件中找到<Routerlink>标签，并添加id参数
<Routerlink ：to="/detail/${item.id}">
p55
详情页-基础数据渲染
封装接口-》调用获取数据-》渲染模板
封装接口在apis/detail.js文件中
import http from '@/utils/http' 
export const getDetailAPI = (id) => {
  return  request({
    url: `/goods/${id}`,
    params: {
      id
    }
  })
}
调用获取数据
import { getDetailAPI } from '@/apis/detail'
import { ref, onMounted } from 'vue'
const detailData = ref({})
const getDetail = async () => {
  const res = await getDetailAPI(route.params.id)
  detailData.value = res.result
}
//使用onMounted钩子函数，在组件挂载完成后调用getDetail函数获取数据
onMounted(() => {
  getDetail()
})
渲染模板
渲染商品详情数据
 <div class="bread-container">
                <el-breadcrumb separator=">">
                    <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
                    <el-breadcrumb-item :to="{ path: `/category/${goods.categories[1].id}` }">{{ goods.categories[1].name }}
                    </el-breadcrumb-item>
                    <el-breadcrumb-item :to="{ path: `/category/sub/${goods.categories[0].id}` }">{{
                        goods.categories[0].name }}
                    </el-breadcrumb-item>
                    <el-breadcrumb-item>{{ goods.name }}</el-breadcrumb-item>
                </el-breadcrumb>
            </div>
<!--错误原因：goods.categories[1]不存在，应该是goods.categories[0]
1.可选链运算符?.，可以避免报错
2.v-if手动控制渲染时机，保证数据存在才能渲染
-->               
设置统计数据
渲染数据在段落区一个个渲染
p56
详情页-热榜区-基础组件封装和数据渲染
封装hot热榜组件-》获取渲染基础数据
在components目录下创建Hot.vue组件准备静态模板
准备基础数据
import { getHotAPI } from '@/apis/detail'
import { ref, onMounted } from 'vue'
//定义数据
const hotList = ref([])
//定义接口函数
const getHot = async () => {
  const res = await getHotAPI()
  hotList.value = res.result
}
//使用onMounted钩子函数，在组件挂载完成后调用getHot函数获取数据
onMounted(() => {
  getHot()
})
在接口文件中定义接口函数
import http from '@/utils/http'
export const getHotAPI = () => {
  return  request({
    url: `/goods/hot`,
    params: {
      id
    }
  })
}
渲染模板
渲染基础数据
<div class="hot-container">
    <div class="title">   
        <h3>热销商品</h3>
    </div>
    <div class="body">
        <div class="item" v-for="item in hotList" :key="item.id">
            <RouterLink :to="`/detail/${item.id}`">
                <img :src="item.picture" alt="" />
                <p class="name">{{ item.name }}</p>
                <p class="price">&yen;{{ item.price }}</p>
            </RouterLink>
        </div>
    </div>
</div>
p57
详情页-热榜区
-》适配不同标题-》适配不同列表内容
//以24小时热榜获取数据渲染模板
import { getHotAPI } from '@/apis/detail'
import { ref, onMounted } from 'vue'
//设计props接收参数，适配不同标题和数据
const props = defineProps({
 hotTitle: {
    type: Number
  }})
  //1.封装接口
  //2.调用接口渲染模板
  const hotList=ref([])
  const route=useRoute()
  const getHot=async ()=>{
    const res=await getHotAPI(
      {
        id:route.params.id,
        type:props.hotType
      }
    )
    
    hotList.value=res.result
  }
  onMounted(()=>{
    getHot()
  })
适配title1. 24小时热销商品
适配title2. 7天热销商品
const TYPEMAP = {
  1: '24小时热销商品',
  2: '7天热销商品'
}
 <!-- 24热榜+专题推荐 -->
                        <div class="goods-aside">
                            <!-- 24小时热榜 -->
                            <GoodHot :type="1" />
                            <!-- 周热榜 -->
                            <GoodHot :type="2" />
p58
详细页-图片预览组件-小图切换大图显示
准备组件静态模板（包括图片数据列表）-》为小图绑定事件，记录当前激活下标值-》通过下标切换大图显示-》通过下标实现激活状态显示
1.在assets目录下创建compents目录，创建ImgPreview.vue组件准备静态模板
2.为小图绑定事件，记录当前激活下标值
//小图切换大图显示
const activeIndex=ref(0)
const enterhander=(i)=>{
  activeIndex.value=i
}
<!--小图列表-->
<ul class="small">
<li v-for="(img,i) in imageList":key="i" @mouseenter="enterhander(i)">
<img :src="img" alt=""/>
</li>
</ul>
有active类名的li标签会有边框激活
&.active {
                border: 2px solid $xtxColor;
            }
active匹配激活
class="{active:i===activeIndex}"//i===activeIndex为true时添加active类名
p59
详情页-图片预览组件-放大镜-滑块跟随移动
思路：获取到当前鼠标在盒子内的相对位置（useMouseInElement），控制滑块跟随鼠标移动（left/top）
1.获取鼠标在盒子内的相对位置
首先导入import {useMouseInElement} from '@vueuse/core'
const target=ref(null)
const {elementX,elementY,isOutside}=useMouseInElement(target)
2.控制滑块跟随鼠标移动
通过计算left/top值，控制滑块跟随鼠标移动
const left=ref(0)
const top=ref(0)
watch([elementX,elementY],(=>{
  console.log('xy变化了')
  //有效范围内控制滑块距离
  //横向
  if(elementX.value>100 && elementX.value<300)
  {
    left.value=elementX.value-100
  }
  //纵向
  if(elementY.value>100 & elementY.value<300)
  {
    top.value=elementY.value-100
  }
  //处理边界
  if(elementX.value<100)
  {
    left.value=0
  }
  if(elementX.value>300)
  {
    left.value=200
  }
  if(elementY.value<100)
  {
    top.value=0
  }
  if(elementY.value>300)
  {
    top.value=200
  }
}))
最后在蒙层小滑块里面修改style样式
<div class="layer" v-show"!isOutside" :style="{left:'${left}px',top:'${top}px'}">
</div>
p61
详情页-图片预览组件-放大镜-大图效果实现
//大图效果实现
const positionX=ref(0)
const positionY=ref(0)
//然后控制大图效果显示
positionX.value=-left.value*2
positionY.value=-top.value*2
//然后在放大镜大图里面修改
<div class="large" :style="[
  {
    backgroundImage:'url(${imageList[acttiveIndex.value]})',
    backgroundPositionX:'${positionX}px',
    backgroundPositionY:'${positionY}px',
  }
]" v-show="!isOutside">
</div>
鼠标移入盒子(isoutside),滑块和大图才显示（v-show）
//优化细节：如果鼠标没有移入盒子里面直接不执行后面的逻辑
if(isOutside.value) return
p62
详情页-图片预览组件-props适配和整体总结
组件props适配
组件中的图片列表不能写死，需要通过props参数把接口数据传入
defineProps({
  imageList:{
    type:Array,
    default:()=>[]
  }
})
在图片预览区域渲染图片列表
<ImageView:image-list="goods.mainPictures" />
p63
详情页-sku组件熟悉使用
sku组件的使用：产出当前用户选择的商品规格，为加入购物车操作提供数据信息
在实际工作中，经常会遇到别人写好的组件，熟悉一个三方组件，首先重点看什么？
props和emit
props决定了当前组件接受什么数据，emit决定了会产出什么数据
在components/Sku.vue组件中，props接收skuList数据，emit产出skuId和skuNum
p64
详情页-通用组件统一注册全局
//把components中的所以组件都进行全局化注册
//通过插件的方式
插件逻辑代码
import imageView from './ImageView/index.vue'
import sku from './Sku/index.vue'
export default {
  install(app)//app为Vue实例
  {
    app.component('ImageView',imageView)
    app.component('Sku',Sku)
  }
}
在main.js中注册插件
import {componentPligin} from '@/components'
app.use(compoentPlugin)
p65
登录-整体认识和路由配置
登录页面主要功能就是表单校验和登录登出业务
在views目录下有一个Login目录，里面有一个index.vue文件，就是登录页面
多模板渲染区分登录状态和非登录状态
在layout/components/Layoutnav.vue文件下设置
<li><a href="javascript:;" @click="$router.push('/login')">跳转到路由配置里面的Login页面
p66
登录-表单校验
为什么需要表达校验？
作业：前端提前校验可以省去一些错误的请求提交，为后端节省压力
表单如何进行校验？
ElemnetPlus表单组件里面内置了表单校验功能，只需要按照组件要求配置必要参数即可
表单校验步骤
1.按照接口字段准备表单对象并绑定
找到登录文件Login中的index.vue文件，找到表单对象
import (ref) from 'vue'
const form = ref({
  account:'',
  password:'',
  agree:true
})
2.按照产品要求准备规则对象并绑定
const rules = {
  account:[
    {
      required:true,message:'账户名不能为空',trigger:'blur'
    },
    {
      required:true,message:'密码不能为空',trigger:'blur'
    },
    {
      min:6,max:16,message:'密码长度要求6-16位',trigger:'blur'
    }
  ]
}
3.指定表单域的校验字段名
在表单组件中
<el-form ref="form" :rules="rules">
<el-form-item prop="account" label="账户名">
<el-form-item prop="password" label="密码">
指定rules属性为rules对象
4.把表单对象进行双向绑定
<el-input v-model="form.account"/>
<el-input v-model="form.password"/>
p67
登录-表单校验-自定义校验规则
ElementPlus表单组件内置了初始的校验配置，应付简单的校验只需要通过配置即可，如果想要定制一些特殊的校验需求，可以使用自定义校验规则，格式如下：
在agree字段上配置自定义校验规则
{
  validator(rule,val,callback)
  =>{
      //自定义校验逻辑

      //value：输出当前的数据
      if(val)
      {
        callback()
      }
      else{
        callback(new Error('请同意协议'))
      }
      //callback：校验处理函数，校验通过调用
  }
}
校验逻辑：如果勾选了协议框，通过校验，如果没有勾选，不通过校验
p68
登录-表单校验-统一校验
整个表单的内容验证
思考：每个表单域都有自己的校验触发事件，如果用户一上来就点击登录怎么办？
在点击登录时需要对所有需要校验的表单进行统一校验
统一校验的步骤
1.定义一个统一的校验方法
const validate=((valid)=>{
      if(valid)
      {
        console.log('submit')
      }
      else{
        console.log('error')
        return false
      }
}
)
2.在点击登录按钮时调用统一校验方法组件中绑定doLogin方法事件
然后获取form实例做统一校验
const router= useRouter()
const formRef=ref(null)
const doLogin=()=>{
  //获取表单数据
  const {account,password} = form.value
  //调用实例方法进行表单校验
  formRef.value.validate(async(valid)=>{
    //valid:所以表单都通过校验，才为true
    console.log(valid)
    if(valid)
    {
      //调用登录接口
    }
  })
}
p69
登录-基础功能实现
表单校验通过-》封装登录接口-》调用登录接口-》提示用户登录成功-》跳转到首页
新增一个use.js文件，用来存放登录相关的业务逻辑

1.//封装所有和用户相关的接口函数
export const LoginAPI = ({account ,password})=>{
  return http({
    url:'/login',
    method:'POST',
    data:{
      account,password
    }
  })
}
2.//调用登录接口
import {LoginAPI} from '@/apis/login'
在Login/index.vue文件中
进行解构赋值
const {account,password}=form.value
//调用登录接口
const res=await LoginAPI({account,password})
//提示用户登录成功，使用ElementPlus的Message组件需要先引入
import { ElMessage } from 'element-plus'
import 'element-plus/theme-chalk/el-message.css'
ElMessage({type:'success',message:'登录成功'})
//跳转到首页，也需要引入useRouter
import { useRouter } from 'vue-router'
//调用useRouter方法
const router=useRouter()
router.replace({path:'/'})
笔记：
当你执行某个操作后需要跳转到一个全新的页面,而不希望通过按钮回到前一个页面时,推荐使用replace，防止用户重复返回到页面里面；如果你希望能够回退到前一个页面，推荐使用push
p70
登录-Pinia管理用户数据
为什么要用Pinia管理数据？
由于用户数据的特殊性，在很多组件中都有可能进行共享，共享的数据使用Pinia管理会更加方便
遵循理念：和数据相关的所以操作（state+action）都放到Pinia中，组件只负责action函数的调用，这样可以让代码更加清晰，更加容易维护
在store目录下创建user.js文件，用来存放用户相关的state和action
//管理用户数据相关
import{deifneStore} from 'pinia'
export const useUserStore=defineStore('user',()=>{
  //1.定义管理用户数据的state
  const userInfo=ref({})
  //2.定义获取接口数据的action函数
  const getUserInfo=async(account,password)=>{
    const res = await loginAPI({account,password})
    UserInfo.value=res.result
  }
  //3.以对象的格式把state和action return出去
  return{
    userInfo,
    getUserInfo
  }
  }))
在login/index.vue文件中
import {useUserStore} from '@/stores/user'

const userStore =  useUserStore()//拿到实例对象
//调用action函数
await userStore.getUserInfo(account,password)
//拿到state数据
console.log(userStore.userInfo.value)
组件触发action-》action函数内部接口获取数据-》用户信息存入
关键代码：
export const useUserStore = defineStore('user', () => {
const cartStore = useCartStore()
// 1. 定义管理用户数据的state
const userInfo = ref({})
// 2. 定义获取接口数据的action函数
const getUserInfo = async ({ account, password }) => {
    const res = await loginAPI({ account, password })
    userInfo.value = res.result
    }
})
// 3. 以对象的格式把state和action return出去
return {
    userInfo,
    getUserInfo
}
await userStore.getUserInfo({ account, password })//触发action函数
p71
登录-Pinia用户数据持久化
持久化用户数据说明
1.用户数据中有一个关键的数据叫做Token（用来标识当前用户是否登录），而Token持续一段时间才会过期
2.Pinia的存储是基于内存的，刷新就丢失，为了保存登录状态就要做到刷新不丢失，需要配合持久化进行存储
目的：保持token不丢失，保持登录状态
最终效果：操作state时会自动把数据在本地的localStorage也存一份，刷新的时候会从localStorage中先取
可以用插件的方式实现，插件代码如下：
npm install pinia-plugin-persistedstate
const pinia = createPinia()
//注册持久化插件
pinia.use(piniaPluginPersistedstate)
在user.js文件中
配置插件
{
  persist：true
}哪个模块需要就在哪里配置
const pinia = createPinia()
//注册持久化插件
pinia.use(piniaPluginPersistedstate)
持久化配置 存入ls
persist:{
  enabled:true,
}
p72
登录-登录和非登录状态下的模板适配
 <!--多模版渲染区分登录状态和非登录状态-->
                <!--适配思路：登录时显示第一块非登录时显示第二块是否有token-->
                <template v-if="userStore.userInfo.token">
                    <li><a href="javascript:;"><i class=" iconfont icon-user"></i>{{userStore.userInfo.account}}</a></li>
                    <li>
                        <el-popconfirm @confirm="confirm" title="确认退出吗?" confirm-button-text="确认" cancel-button-text="取消">
                            <template #reference>
                                <a href="javascript:;">退出登录</a>
                            </template>
                        </el-popconfirm>
                    </li>
                    <li><a href="javascript:;">我的订单</a></li>
                    <li><a href="javascript:;">会员中心</a></li>
                </template>
                <template v-else>
                    <li><a href="javascript:;" @click="$router.push('/login')">请先登录</a></li>
                    <li><a href="javascript:;">帮助中心</a></li>
                    <li><a href="javascript:;">关于我们</a></li>
                </template>
引用userStore.userInfo.token判断是否登录，如果有token，显示登录状态，没有token显示登录按钮
多模板适配的通用思路
有几个需要适配的模板就准备几个template片段，通过条件渲染控制显示即可
<template v-if="条件一">
    <!--需要显示的内容-->
</template>
<template v-else-if="条件二">
    <!--需要显示的内容-->
</template>
<template v-else>
    <!--需要显示的内容-->
</template>
p73
登录-请求拦截器携带Token
Token作为用户标识，在很多个接口中都需要携带Token才可以正确获取数据，所以需要在接口调用时携带Token。为了统一控制采取请求拦截器携带的方案
如何配置
Axios请求拦截器可以在接口正式发起之前对请求参数做一些事情，通常Token数据会被注入到请求header中，格式安装后端要求的格式进行拼接处理
在utils目录下有http.js文件，里面封装了axios请求，可以对请求做一些统一的处理，比如请求拦截器
instance.interceptors.request.use(config =>{
  const userStore = useUserStore()
  const token = userStore.userInfo.token
  if(token)
  {
    config.headers.Authorization = 'Bearer ${token}'
  }
  return config
},e=> Promise.reject(e))
p74
登录-退出登录功能实现
点击退出登录弹出确认框，确认退出，调用退出登录接口，清除用户信息，跳转到登录页面
绑定点击事件
<el-popconfirm @confirm="confirm" title="确认退出吗？" confirm-button-text="确认" cancel-button-text="取消">
调用回调函数
const confirm=()=>{
  console.log('confirm退出')
}
//退出时清除用户信息
const clearUserInfo=()=>{
  userInfo.value={}
  //执行清除购物车的action
  cartStore.clearCart()
}
回到layoutNav.vue文件中
//退出登录业务逻辑实现

引入useUserStore和useCartStore
import {useUserStore} from '@stores/user'
import {useRouter} from 'vue-router'
const userStore = useUserStore()
const cartStore = useCartStore()
//清除用户信息，触发action函数
userStore.clearUserInfo()
//跳转登录页面
router.push('/login')
p75
登录-Token失效401拦截处理
业务背景：Token的有效性可以保持一定的时间，如果用户一段时间不做任何操作，Token就会失效，
使用失效的Token再去请求一些接口，这些接口就会报401状态码错误，需要我们做额外处理
解决方案-在axios响应拦截器做统一处理
失败回调中拦截401-》清除掉过期的用户信息，跳转到登录页面
在utils目录下有http.js文件，里面封装了axios请求，可以对请求做一些统一的处理，比如响应拦截器
//axios响应式拦截器
http.interceptors.response.use(res=>res.data,e=>{
  //从pinia里面获取token数据
  const userStore=useUserStore()
  //错误统一提示
  ElMessage({
    type:'warning',
    message:e.response.data.message

  })
  //401 Token失效401拦截处理
  //1.清除本地用户数据
  //2.跳转登录页面
  if(e.response.status===401)
  {
    userStore.clearUserInfo()
    router.push('/login')
  }
  return Promise.reject(e)
})
p76
购物车-流程梳理和本地加入购物车实现
购物车业务逻辑梳理拆解
判断是否登录：
1.本地购物车操作(所以操作不走接口直接操作pinia中的本地购物车列表)
2.接口购物车操作(所以操作直接走接口，操作完毕获取购物车列表更新本地购物车列表)
1.整个购物车的实现分为俩个大分支，本地购物车操作和接口购物车操作
2.由于购物车数据的特殊性，采取pinia管理购物车列表数据并添加持久化缓存
本地购物车实现
1.封装cartStore(state,action)
=>
2.组件点击添加按钮
=》选择了商品规格和数量
=》调用action函数添加商品到购物车列表=》添加过原count+1，没有过添加到列表直接push跳转到购物车列表
未选择规格=》提示用户选择规格
创建views/CartList/index.vue文件
//封装购物车模板
import {defineStore} from 'pinia'
import {ref} from 'vue'
export const useCartStore=defineStore('cart',()=>{
  //1.定义state-cartList
  const cartList=ref([])
  //2.定义action-addtocart
  const addCart=()=>
  {
    //添加购物车操作
  }
  return{
    cartList,
    addCart
  }
})
//组件点击添加按钮
 <!-- 按钮组件 -->
       <div>
          <el-button size="large" class="btn"@click="addCart">
                         加入购物车
          </el-button>
       </div>
//sku规格被操作时
let skuObj={}
const skuChange=(sku)=>{
  console.log(sku)
  skuObj=sku
}
//count
const count=ref(1)
const countChange=>(count)=>{
  console.log(count)
}
//添加购物车
const addCart=()=>{
  if(skuObj.skuId)
  {
    //规则已经选择 触发action
    cartStore.addCart({
      id:goods.value.id,
      name:goods.value.name,
      picture:goods.value.mainPictures[0],
      price:goods.value.price,
      count:count.value,
      skuId:skuObj.skuId,
      attrsText:skuObj.attrsText,
      selected:true
    })
  }else{
    //规则已经选择 触发action
    ElMessage.warning('请选择规格')
  }
}
p77
购物车-本地-头部购物车列表渲染
准备头部购物车组件-》从Pinia中获取数据渲染列表
创建layout/component/headercart.vue文件
在layoutheader.vue文件中加入import HeaderCart from '@./HeaderCart.vue'
在template中加入<HeaderCart/>充当头部购物车
p78
购物车-本地-头部购物车实现删除功能
在cartstore.js文件中定义删除商品的action函数
const clearCart=()=>{
  caetList.value=[]
}
//删除购物车
const delCart=async(skuId)=>
{
  if(isLogin.value)
  {
    //调用接口实现接口购物车中的删除功能
    await delCartAPI([skuId])
    updateNewList()
  }else
  {
    //思路：
    //1.找到要删除项的下标值-splice
    //2.使用数组的过滤方法-filter
    const idx=cartList.value.findIndex((item)=>skuId===item.skuId)
    cartList.value.splice(idx,1)

  }
  return {
    cartList,
    addCart,
    delCart
  }
}
然后再HeaderCart.vue文件中调用action函数实现删除功能
<i class="el-icon-delete" @click="delCart(item.skuId)"></i>
p79
购物车-本地-头部购物车统计计算
//是否全选计算属性
const isAll=computed(()=>cartList.value.every((item)=>item.selected))
//计算属性
//1.总的数量 
const allCount=computed(()=>carList.value.reduce((a,c)=>a+c.count,0))
//2.总的价格
const allPrice=computed(()=>cartList.value.reduce((a,c)=>a+c.price*c.count,0))
//3.已选择数量
const selectedCount=computed(()=>cartList.value.filter((item)/=>item.selected).reduce(a,c)=>a+c.count,0)
//4.已选择商品价钱合计
const selectedCount=computed(()=>cartList.value.filter(item=>item.seleceted).reduce(a,c)=>a+c.price*c.count,0)
然后在return中
return {
  allCount,
        allPrice,
        isAll,
        selectedCount,
        selectedPrice,
}
然后在template中渲染
<p>共{{cartStore.allCount}}件商品，总价{{cartStore.allPrice}}元</p>
