<template>
  <div class="container">
    <!-- 轮播图区域 -->
    <slice-pic :slicePicList="slicePicList" :isfull="true"></slice-pic>

    <ul class="mui-table-view mui-grid-view mui-grid-9">
      <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-4">
        <router-link to="/home/newslist">
          <img src="../images/menu1.png" />
          <div class="mui-media-body">新闻资讯</div>
        </router-link>
      </li>
      <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-4">
        <router-link to="/home/photolist">
          <img src="../images/menu2.png" />
          <div class="mui-media-body">图片分享</div>
        </router-link>
      </li>
      <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-4">
        <router-link to="/home/goodslist">
          <img src="../images/menu3.png" />
          <div class="mui-media-body">商品购买</div>
        </router-link>
      </li>
      <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-4">
        <a href="#">
          <img src="../images/menu4.png" />
          <div class="mui-media-body">留言反馈</div>
        </a>
      </li>
      <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-4">
        <a href="#">
          <img src="../images/menu5.png" />
          <div class="mui-media-body">视频专区</div>
        </a>
      </li>
      <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-4">
        <a href="#">
          <img src="../images/menu6.png" />
          <div class="mui-media-body">联系我们</div>
        </a>
      </li>
    </ul>
  </div>
</template>

<script>
import { Toast } from "mint-ui";
import slicePic from '../components/subcomponents/SlicePic.vue'

export default {
  data() {
    return {
      slicePicList: [] //轮播图数组
    };
  },
  methods: {
    //获取轮播图
    getSlicePic() {
      this.$http.get("api/getlunbo").then(result => {
        console.log(result.body);
        if (result.body.status == 0) {
          this.slicePicList = result.body.message;
        } else {
          Toast("获取加载轮播图失败");
        }
      });
    }
  },
  components:{
    slicePic
  },
  created() {
    this.getSlicePic();
  }
};
</script>

<style scoped lang="scss">
.container {

  .mui-grid-view.mui-grid-9 {
    background-color: white;
    border: none;

    img {
      width: 60px;
      height: 60px;
    }

    .mui-media-body {
      font-size: 13px;
    }
  }
}
</style>