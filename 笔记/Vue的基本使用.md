### 一、Vue的基本使用



###	二、Vue中包含的模块

##### 1、Vue的核心

1. name 
   * 组件的名称

2. data

   - 保存Vue中常用的数据

     ```javascript
     data() {
         return {
             weaponData: [],
             selectIndex: 0
         }
     }
     ```

3. methods

   - 在这里定义Vue中常用的方法

     ```javascript
     methods: {
         itemClick(index) {
             this.selectIndex = index
         },
     }
     ```

4. components

   - 组件列表，在这里注册子组件

     ```javascript
     import ItemList from 'components/items/ItemList'
     import Item from 'components/items/Item'
     import ShowWeapon from './ShowWeapon'
     import GButton from 'components/button/GButton'
     
     components: {
         ItemList,
         Item,
         ShowWeapon,
         GButton
     }
     ```

5. computed

   - 计算属性，使用计算属性后，可以再页面中使用{{ }}直接引用

     

     ```javascript
     export default {
         name: 'ShowWeapon',
         data() {
             return {
                 titleBgColors: ["#72778B", "#2A9072", "#5180CC", "#A256E1", "#BD6932"],
                 propBgColors: ["#525863", "#4B5C5F", "#515777", "#605786", "#735854"]
             }
         },
        
         computed: {
             titleBgColor() {
                 var index = this.weapon.quality - 1;
                 return this.titleBgColors[index];
             },
             propBgColor() {
                 var index = this.weapon.quality - 1;
                 return this.propBgColors[index];
             }
         },
     }
     
     
     ```

   ```html
   <div :style="{backgroundColor:propBgColor}">
       内容
   </div>
   ```

6. mixins

   - 混入

##### 2、vue的生命周期函数

1. created 组件创建的时候调用

   * 发起异步请求从服务器获取数据，将数据保存到data中。

   * 此时页面还没有进行渲染，不适合做获取页面dom元素，修改样式等操作。

2. mounted 组件显示的时候调用

3. destory 组件销毁的时候调用

4. update 页面更新的时候调用

##### 3、Vue中常用的依赖模块

1. vue-router 页面的前端路由

   ```javascript
   // 配置路由相关的信息
   import VueRouter from 'vue-router'
   import Vue from 'vue'
   
   //路由懒加载
   const Material = () => import('@/view/material/Material')
   const Weapon = () => import('@/view/weapon/Weapon')
   
   // 1.通过Vue.use(插件), 安装插件
   Vue.use(VueRouter)
   
   // 2.创建VueRouter对象
   const routes = [
       {
           path: '',
           // redirect重定向
           redirect: '/weapon'
       },
       {
           path: '/material',
           component: Material,
       },
       {
           path: '/weapon',
           component: Weapon,
       },
   ]
   
   const router = new VueRouter({
       // 配置路由和组件之间的应用关系
       routes,
       mode: 'history',
       linkActiveClass: 'active'
   }) 
   
   router.beforeEach((to, from, next) => {
       // // console.log(to);
       // // console.log(from);
       // document.title = to.matched[0].meta.title
       next();
   })
   
   // 3.将router对象传入到Vue实例
   export default router
   ```

2. veux 页面公共数据的存储与管理

   ```javascript
   mport Vue from 'vue'
   import Vuex from 'vuex'
   
   Vue.use(Vuex)
   
   const moduleA = {
       state: {
           name: 'aaaa'
       },
       getters: {
           fullname(state) {
               return state.name + " bbbbbbbb"
           }
       },
       mutations: {
           actionUpdateModuleName(state, name) {
               state.name = name
           }
       }
   }
   
   const store = new Vuex.Store({
       state: {
           count: 0,
           name: '张三',
           students: [
               {
                   id: 101,
                   name: '刘备',
                   age: 22
               },
               {
                   id: 102,
                   name: '关羽',
                   age: 19
               },
               {
                   id: 103,
                   name: '张飞',
                   age: 17
               },
               {
                   id: 104,
                   name: '赵云',
                   age: 15
               }
           ]
       },
       mutations: {
           increment(state) {
               state.count++
           },
           decrement(state) {
               state.count--
           },
           addCount(state, count) {
               state.count += count
           },
           updateName(state, name) {
               state.name = name
           }
       },
       getters: {
           //属性读取
           more18ageStus(state) {
               return state.students.filter(s => s.age > 18);
           },
           //带参数的属性读取
           moreAgeStus(state) {
               return function (age) {
                   return state.students.filter(s => s.age > age);
               }
           }
       },
       actions: {
           actionUpdateName(context, pyload) {
               console.log(pyload);
   
               return new Promise(function (resolve) {
                   setTimeout(function () {
                       context.commit('updateName', pyload)
                       resolve('name修改完成')
                   }, 1000)
               });
           }
       },
       modules: {
           a: moduleA
       }
   })
   
   export default store
   ```

3. axios vue中常用的异步请求模块，类似 JQuery中的ajax

   - get请求

     ```javascript
     // 上面的请求也可以这样做
     axios.get('/user', {
         params: {
           ID: 12345
         }
       })
       .then(function (response) {
         console.log(response);
       })
       .catch(function (error) {
         console.log(error);
       });
     ```

   - post请求

     ```javascript
     axios.post('/user', {
         firstName: 'Fred',
         lastName: 'Flintstone'
       })
       .then(function (response) {
         console.log(response);
       })
       .catch(function (error) {
         console.log(error);
       });
     ```









​	

