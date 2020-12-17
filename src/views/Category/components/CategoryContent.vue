<!--  -->
<template>
  <div class="category-content">
    <div class="category-left">
      <ul>
        <li
          :class="{active:catId==categoryLeftData.cat_id}"
          v-for="categoryLeftData in categoryLeftDatas"
          :key="categoryLeftData.cat_id"
          @click="changeTab(categoryLeftData.cat_id)"
        >{{categoryLeftData.cat_name}}</li>
      </ul>
    </div>
    <div v-if="!showloading">
      <Loding />
    </div>
    <div class="category-right" v-else>
      <!-- <div v-if="showText" style="text-align:center; line-height:3rem">下拉刷新</div> -->
      <div :class="{'category-right':true}" id="wrapper">
        <div class="category-right-content" id="content" :style="{height:OH}" :class="{'top':flag}">
          <div class="ad" ref="ad">
            <img :src="this.$store.state.ad" alt />
          </div>
          <ul class="category-list" ref="list">
            <li v-for="categoryRightData in categoryRightDatas" :key="categoryRightData.cat_id">
              <h3>
                <span>-</span>
                {{categoryRightData.cat_name}}
                <span>-</span>
              </h3>
              <ul class="child">
                <li v-for="category in categoryRightData.child" :key="category.cat_id">
                  <router-link :to="'/categorylist/'+category.cat_id">
                    <img :src="category.touch_icon" alt />
                    <h4>{{category.cat_name}}</h4>
                  </router-link>
                </li>
              </ul>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）
//例如：import 《组件名称》 from '《组件路径》';
import BScroll from "better-scroll"; //下载 cnpm install better-scroll --save
import Loding from "@/components/Loading";
export default {
  //import引入的组件需要注入到对象中才能使用
  components: {
    Loding,
  },
  data() {
    //这里存放数据
    return {
      showText: false,
      flag: false,
      arr: ["858", "6", "8", "3", "4", "5", "860"],
      index: 0,
      OH: "1500px",
    };
  },
  watch: {
    categoryRightDatas() {
      this.$nextTick(() => {
        this.scrollPage();
      });
    },
  },
  methods: {
    changeTab(cat_id) {
      console.log(cat_id);
      this.cat_id = cat_id;
      this.flag = true;
      this.$store.dispatch("actChangeCategoryRight", cat_id);
    },
    scrollPage() {
      this.rightScroll = new BScroll("#wrapper", {
        pullUpLoad: true,
        scrollbar: false,
        pullDownRefresh: true,
        click: true, //解决阻止默认行为的问题
      });
      this.rightScroll.on(
        "scroll",
        (obj) => {
          if (obj.y > 30 && obj.y < 100) {
            this.showText = true;
          } else {
            this.showText = false;
          }
          if (obj.y > 100) {
            let index = this.arr.findIndex((val) => {
              return val == this.$store.state.cat_id;
            });

            if (index) {
              console.log(this.index);
              this.$store.dispatch(
                "actChangeCategoryRight",
                this.arr[index - 1]
              );
              this.rightScroll.refresh();
            } else {
              this.$store.dispatch("actChangeCategoryRight", this.arr[index]);
            }
          }

          this.scrollTop = Math.abs(obj.y); //Math.abs() 绝对值
          //
          // console.log(this.scrollTop);
          if (document.querySelector("#content")) {
            this.oHeight =
              document.querySelector("#content").scrollHeight -
              document.querySelector("#wrapper").offsetHeight;
          }

          // console.log(this.oHeight);
          // console.log(this.scrollTop);
          if (this.scrollTop > this.oHeight + 100) {
            // console.log(this.$store.state.cat_id);

            let index = this.arr.findIndex((val) => {
              return val == this.$store.state.cat_id;
            });

            // console.log(this.index);
            this.$store.dispatch("actChangeCategoryRight", this.arr[index + 1]);

            this.rightScroll.refresh();
          }
        },
        false
      );
    },
  },
  computed: {
    categoryLeftDatas() {
      return this.$store.state.categoryLeftDatas;
    },
    categoryRightDatas() {
      return this.$store.state.categoryRightDatas;
    },
    showloading() {
      return this.$store.state.showLoading;
    },
    catId() {
      return this.$store.state.cat_id;
    },
  },
  mounted() {
    if (this.$store.state.showLoading) {
      console.log(this.$refs.ad.offsetHeight);
      console.log(this.$refs.list.offsetHeight);
      // this.OH =
      //   this.$refs.ad.offsetHeight + this.$refs.list.offsetHeight + "px";
    }
    this.$store.dispatch("actChangeCategoryLeft");
    this.$store.dispatch("actChangeCategoryRight", "858");
  },
  updated() {
    if (this.$store.state.showLoading) {
      console.log(this.$refs.ad.offsetHeight);
      console.log(this.$refs.list.offsetHeight);
      // this.OH =
      //   this.$refs.ad.offsetHeight + this.$refs.list.offsetHeight + "px";
    }
  },
};
</script>
<style lang="less">
.category-content {
  margin: 4.4rem 0 4.9rem 0;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  //   background: #ccc;
  width: 100%;
  height: 100%;
  height: calc(100% - 9.3rem);
  display: flex;
  overflow: hidden;
  .category-left {
    height: 100%;
    background: #efefef;
    ul {
      width: 8.4rem;
      li {
        height: 3.8rem;
        text-align: center;
        line-height: 3.8rem;
        font-size: 1.3rem;
      }
      .active {
        color: red;
        background: #fff;
      }
    }
  }
  .category-right {
    width: 100%;
    height: 100%;
    overflow-y: hidden;
    .top {
      transform: translateY(0);
    }
    .category-right-content {
      .ad {
        width: 100%;
        margin-top: 1rem;
        img {
          width: 90%;
          margin-left: 5%;
        }
      }
      .category-list {
        li {
          a {
            display: block;
            // background: red;
          }
          h3 {
            font-size: 1.4rem;
            height: 4.4rem;
            line-height: 4.4rem;
            color: #666;
            text-align: center;
            font-weight: normal;
            span {
              color: #ccc;
            }
          }
          .child {
            display: flex;
            flex-wrap: wrap;
            text-align: center;
            li {
              width: 33%;
              height: 8.2rem;
              margin-bottom: 1rem;
              img {
                width: 5.2rem;
                height: 5.2rem;
              }
              h4 {
                font-size: 1.2rem;
                font-weight: normal;
                color: #999;
                margin-top: 1rem;
              }
            }
          }
        }
      }
    }
  }
}
</style>