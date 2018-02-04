<template>
  <div class="index">
    <view-box>
       <x-header slot="header" class="header" :left-options="{showBack: false}">
         <div slot="left">直播</div>
         <div>网易</div>
         <div slot="right">搜索</div>
       </x-header>

        <div class="main">
          <scroller lock-y>
              <div class="tab">
                  <tab>
                    <tab-item selected>新闻</tab-item>
                    <tab-item>体育</tab-item>
                    <tab-item>科技</tab-item>
                    <tab-item>直播</tab-item>
                    <tab-item>汽车</tab-item>
                    <tab-item>娱乐</tab-item>
                  </tab>
              </div>
          </scroller>
          <swiper :list="list" v-model="list_index" loop auto></swiper>
          
          <marquee>
            <marquee-item v-for="mar in marquee" class="marquee-news">{{mar.title}}</marquee-item>
            
          </marquee>

          <panel header="图文组合列表" :list="list2"></panel>
          
        </div>
       
        <tabbar slot="bottom">
          <tabbar-item>
            <img slot="icon" src="../assets/1.png">
            <span slot="label">首页</span>
          </tabbar-item>
          <tabbar-item>
            <img slot="icon" src="../assets/2.png">
            <span slot="label">推荐</span>
          </tabbar-item>
          <tabbar-item>
            <img slot="icon" src="../assets/3.png">
            <span slot="label">我的</span>
          </tabbar-item>
          
        </tabbar>
       
     </view-box>
  </div>
</template>

<script>
import { ViewBox,XHeader,Tabbar, TabbarItem,Tab, TabItem,Scroller,Swiper,Marquee, MarqueeItem,Panel } from 'vux'

export default {
  components: {
    ViewBox,
    XHeader,
    Tabbar,
    TabbarItem,
    Tab,
    TabItem,
    Scroller,
    Swiper,
    Marquee,
    MarqueeItem,
    Panel
  },
  created(){
    this.$jsonp('http://3g.163.com/touch/jsonp/sy/recommend/0-9.html').then(data => {
      console.log(data);
      // 轮播图的数据
      this.list=data.focus.filter(item => {
        return item.addata==null;
      }).map(item => {
        return {
          url:item.link,
          img:item.picInfo[0].url,
          title:item.title
        }
      })
      // 滚动新闻列表的数据
      this.marquee=data.news.filter(item => {
        return item.addata==null;
      }).map(item => {
        return {
          
          title:item.title
        }
      })
      // 首屏新闻列表的数据
      this.list2=data.list.filter(item => {
        return item.addata==null;
      }).map(item => {
        return {
          src:item.picInfo[0].url,
          title:item.title,
          desc:item.digest,
          url:item.link
        }
      })
    })
  },
  data () {
    return {
      list: [],
      list_index:0,
      list2:[],
      marquee:[]
      
    }
  }
}
</script>

<style>
html, body {
    height: 100%;
    width: 100%;
    overflow-x: hidden;
  }
  .index{
    height: 100%;
  }
  .header{
    position: absolute!important;
    width: 100%;
    left: 0;
    top: 0;
    z-index: 1000
  }
  .main{
    padding-top: 50px;
    

  }
  .tab{
    width: 300%;
  }
  .marquee-news{
    padding: 10px 8px 0px;
    color: #999;
  }
</style>
