<template>
  <div id="app">
      <ele-header :seller="seller"></ele-header>
      <ul class="nav">
          <li><router-link to="/">商品</router-link></li>
          <li><router-link to="/comments">评论</router-link></li>
          <li><router-link to="/seller">商家</router-link></li>
      </ul>
      <router-view></router-view>
  </div>
</template>

<script>
//引入重置的css文件
import "./assets/css/reset.css"

import Header from './components/Header/Header'
//直接导入这个json
// import ele from '../data.json';
// console.log(ele);

export default {
    data(){
        return {
            seller : {} //商家信息
        }
    },
    created(){
        let ele = require('../data.json');
        this.seller = ele.seller;
    },
    name: 'app',
    components : {
      "ele-header" : Header
  }
}
</script>

<style lang="less" scoped>
    .nav {
        height: 40px;
        line-height: 40px;
        display: flex;
        justify-content: space-around;
        border-bottom: 1px solid rgba(7,17,27,.1);
        a{
            color : rgb(77,85,93);
            font-size: 14px;
            text-decoration: none;
            &.router-link-exact-active{
                color:rgb(240,20,20);
            }
        }
    }
</style>
