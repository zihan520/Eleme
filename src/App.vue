<template>
  <div id="app">
    <v-header v-bind:seller="seller"></v-header>
    <div class="tab">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <router-view></router-view>
    <!-- 如果将app里面的seller传递到goods ，ratings，seller之后，就不需要在使用axios获取数据了 -->
    <!-- <router-view :seller="seller"></router-view> -->
  </div>
</template>

<script>
import Header from './components/header/Header.vue'
import axios from 'axios'

const ERR_OK = 0;

export default {
  data() {
    return {
      seller: {}
    }
  },
  created(){
    axios.get('static/data.json').then((res)=>{
        this.seller = res.data.seller;
        console.log(this.seller);
    })
  },
  components: {
    'v-header': Header
  }
}
</script>

<style>
    .tab{
      display:flex;
      width:100%;
      height:40px;
      line-height:40px;
      border-bottom: 1px solid rgba(7,17,27,0.1);
    }
    .tab-item{
      flex:1;
      text-align:center;
    }
    .tab-item .router-link{
      display: block;
      font-size: 14px;
      color: rgb(77,85,93);
    } 
    a.active{
      color: rgb(240,20,20);
    }  
</style>
