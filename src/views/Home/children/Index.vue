<!--  -->
<template>
  <div class="index-page" v-infinite-scroll="loadMore">
    <Loading v-if="isShowLoading" />
    <div class="index-header" :style="colorObj"></div>
    <div class="index-swiper">
      <!-- changeColor 指的是Swiper组件$emit广播出来的事件 -->
      <!-- getColor Index组件中定义的方法 -->
      <Swiper @changeColor="getColor" />
    </div>

    <div
      style="height:8.8rem; background:red;position:fixed;z-index:98;left:0; top:0; width:100%"
      v-if="!showSwiper"
    ></div>

    <div>
      <QuickNav />
    </div>
    <div>
      <HomeNews />
    </div>
    <div>
      <ul class="tabs">
        <li
          v-for="(tabsData,index) in tabsDatas"
          :key="index"
          @click="changeTab(index,tabsData.url,tabsData.type)"
        >
          <h3>{{tabsData.title}}</h3>
          <h6 :class="{active:activeIndex==index}">{{tabsData.tips}}</h6>
        </li>
      </ul>
      <List :listdatas="listdatas" />
    </div>
    <div style="height:5rem"></div>
  </div>
</template>

<script>
//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）
//例如：import 《组件名称》 from '《组件路径》';
import Swiper from "@/components/Swiper";
import QuickNav from "../components/QuickNav";
import HomeNews from "../components/HomeNews";
import Loading from "@/components/Loading";
import { getHomeList } from "@/api/api";
console.log(getHomeList);
//需要下载  cnpm install axios --save
// import Axios from "axios";
import List from "@/components/List";
export default {
  //import引入的组件需要注入到对象中才能使用
  components: {
    Swiper,
    QuickNav,
    HomeNews,
    List,
    Loading,
  },
  data() {
    //这里存放数据
    return {
      activeIndex: 0,
      showSwiper: true,
      tabsDatas: [
        {
          id: 1,
          title: "精选",
          tips: "为你推荐",
          type: "is_best",
          url: "/goods/type_list",
        },
        {
          id: 2,
          title: "社区",
          tips: "新奇好物",
          type: "",
          url: "/discover/find_list",
        },
        {
          id: 3,
          title: "新品",
          tips: "潮流上新",
          type: "is_new",
          url: "/goods/type_list",
        },
        {
          id: 4,
          title: "热卖",
          tips: "火热爆款",
          type: "is_hot",
          url: "/goods/type_list",
        },
      ],
      colorObj: {
        background: "#ec5151",
      },
      colorArr: [
        {
          background: "red",
        },
        {
          background: "#409eff",
        },
        {
          background: "rgb(2,131,121)",
        },
      ],
      listdatas: [],
      page: 1,
      size: 10,
      type: "is_best",
      isShowLoading: false,
      url: "/goods/type_list",
    };
  },
  methods: {
    getColor(data) {
      // console.log(data); //data是子组件广播出来的数据
      this.colorObj = this.colorArr[data];
    },
    async getBestDatas(url, page, size, type) {
      this.isShowLoading = true;
      let result = await getHomeList(url, {
        page: page,
        size: size,
        type: type,
      });
      console.log(result.data);
      if (result.data) {
        let resultArr = result.data;
        this.listdatas = this.listdatas.concat(resultArr);
        this.isShowLoading = false;
      } else if (result.data == "undefined") {
        alert("网络出现故障");
      }
    },
    changeTab(index, url, type) {
      this.page = 1;
      this.listdatas = [];
      this.activeIndex = index;
      this.url = url;
      this.type = type;
      this.getBestDatas(url, this.page, this.size, type);
    },
    loadMore() {
      this.getBestDatas(this.url, this.page, this.size, this.type);
      console.log("-----------123456");
      this.page++;
    },
    scrollPage() {
      console.log(document.documentElement.scrollTop);
      if (document.documentElement.scrollTop > 200) {
        this.showSwiper = false;
      } else if (document.documentElement.scrollTop < 200) {
        this.showSwiper = true;
      }
    },
  },
  mounted() {
    this.getBestDatas(this.url, this.page, this.size, this.type);
    console.log(this.$refs.home);
    // let that = this;
    window.addEventListener("scroll", this.scrollPage, false);
  },
  beforeDestroy() {
    window.removeEventListener("scroll", this.scrollPage, false);
  },
};
</script>
<style lang="less">
.index-page {
  .index-header {
    background: red;
    height: 14.8rem;
    border-bottom-right-radius: 2rem;
    border-bottom-left-radius: 2rem;
    transition: all 0.5s;
  }
  .index-swiper {
    position: relative;
    top: -5rem;
  }
  .tabs {
    width: 100%;
    height: 6rem;
    display: flex;
    justify-content: center;
    align-items: center;
    li {
      flex: 1;
      text-align: center;
      border-right: 1px solid #efefef;
      &:last-child {
        border-right: 0px solid #efefef;
      }
      h3 {
        font-size: 1.4rem;
        color: #666;
        margin-bottom: 3px;
      }
      h6 {
        font-size: 1.2rem;
        color: #999;
        font-weight: normal;
      }
      .active {
        color: #fff;
        background: red;
        width: 80%;
        margin-left: 10%;
        border-radius: 10px;
      }
    }
  }
}
</style>