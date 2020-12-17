<!--  -->
<template>
  <div>
    <div class="home-news">
      <div class="title-img">
        <img :src="titleimg" alt />
      </div>
      <ul ref="news" :class="{trans:flag}">
        <li v-for="(news,index) in newsData" :key="index">{{news.title}}</li>
      </ul>
      <div class="arr">
        <i class="iconfont icon-jiantou2"></i>
      </div>
    </div>
    <h2>{{hours}}-{{mins}}-{{sec}}</h2>
  </div>
</template>

<script>
//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）
//例如：import 《组件名称》 from '《组件路径》';

export default {
  //import引入的组件需要注入到对象中才能使用
  components: {},
  data() {
    //这里存放数据
    return {
      titleimg: require("@/assets/images/title.png"),
      newsData: [
        {
          id: 1,
          title: "新闻001",
        },
        {
          id: 2,
          title: "新闻002",
        },
        {
          id: 3,
          title: "新闻003",
        },
      ],
      timer: null,
      flag: false,
      hours: 0,
      mins: 0,
      sec: 0,

      timer2: null,
    };
  },
  methods: {
    scrollNewsFn() {
      //   console.log(this.$refs.news);
      let oUl = this.$refs.news; //获取ul  dom元素
      oUl.style.top = "-5rem";
      this.flag = !this.flag;
      setTimeout(() => {
        this.newsData.push(this.newsData[0]);
        this.newsData.shift();
        oUl.style.top = "0rem";
        this.flag = !this.flag;
      }, 500);
    },
    miaoshaFn() {
      //当前时间
      var nowTime = new Date();
      //   console.log(nowTime);
      //   目标时间
      var targetTimer = new Date("Fri Nov 19 2020 11:23:01 GMT+0800");
      //   console.log(targetTimer - nowTime);
      //时间差 毫秒数
      var oTime = (targetTimer - nowTime) / 1000; //单位换算成秒
      //算出小时
      this.hours = parseInt((oTime % (60 * 60 * 24)) / 3600);
      //算出分钟
      //(oTime%(60*60*24))%3600/60
      this.mins = parseInt(((oTime % (60 * 60 * 24)) % 3600) / 60);
      //算出秒
      //oTime%(60*60*24)%3600%60
      this.sec = parseInt(((oTime % (60 * 60 * 24)) % 3600) % 60);
      //console.log(this.hours - this.mins - this.sec);
    },
  },
  mounted() {
    this.timer = setInterval(this.scrollNewsFn, 2000);
    this.timer2 = setInterval(this.miaoshaFn, 1000);
  },
  destroyed() {
    //销毁组件
    clearInterval(this.timer);
    clearInterval(this.timer2);
  },
};
</script>
<style lang="less">
.home-news {
  width: 100%;
  margin: 0 10px;
  width: calc(100% - 20px);
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 5rem;
  border-top: 1px solid #efefef;
  border-bottom: 1px solid #efefef;
  overflow: hidden;
  position: relative;
  .title-img {
    width: 20%;
    height: 3rem;
    // margin-top: 1rem;
    img {
      height: 3rem;
    }
  }
  .trans {
    transition: all 0.5s linear;
  }
  ul {
    position: absolute;
    left: 20%;
    height: 5rem;
    width: 70%;
    margin-left: 2rem;
    li {
      line-height: 5rem;
      height: 5rem;
      font-size: 1.4rem;
    }
  }
  .arr {
    width: 10%;
    text-align: right;
  }
}
</style>