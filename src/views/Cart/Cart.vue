<!--  -->
<template>
  <div class="cart-page">
    <div class="cart-header">
      <div class="title">购物车</div>
    </div>
    <div class="cart-wrap">
      <div class="cart-list" v-if="cartData.length>0">
        <ul>
          <li v-for="(cart,index) in cartData" :key="index">
            <div class="select" @click="changSelectStatus(cart,index)">
              <i class="iconfont icon-danxuankuang" v-if="!cart.isSelect"></i>
              <i class="iconfont icon-danxuankuangxuanzhong" style="color:#f44" v-else></i>
            </div>
            <div class="cart-content" @click="toDetail(cart.goods_id)">
              <div class="cart-img">
                <img :src="cart.goods_thumb" alt />
              </div>
              <div class="cart-text">
                <div class="cart-title">{{cart.goods_name}}</div>
                <div class="cart-text-wrap">
                  <div class="price">{{cart.shop_price}}</div>
                  <div class="num-wrap">
                    <div class="num">
                      <a href="javascript:;" @click.stop="jianCart(index)">-</a>
                      <input type="text" v-model="cart.value" />
                      <a href="javascript:;" @click.stop="addCart(index)">+</a>
                    </div>
                    <i class="iconfont icon-shoucang1"></i>
                    <i class="iconfont icon-shanchu1" @click.stop="deleCart(index)"></i>
                  </div>
                </div>
              </div>
            </div>
          </li>
        </ul>
      </div>
      <!-- 购物车没有数据 -->
      <div class="nocart" v-else>
        <i class="iconfont icon-gouwuche"></i>
        <div>
          购物车空空的，
          <router-link to="/home/index">去逛逛</router-link>
        </div>
      </div>
    </div>

    <div class="cart-footer" v-if="cartData.length>0">
      <div class="selectAll" @click="changeSelectAllStatus">
        <i class="iconfont icon-danxuankuang" v-if="!selectAll"></i>
        <i class="iconfont icon-danxuankuangxuanzhong" style="color:#f44" v-else></i>
        <span>全选</span>
      </div>
      <div class="totalPrice">
        <div class="price">
          合计：
          <span>{{total.toFixed(2)}}</span>
        </div>
        <div class="tips">
          (不含运费，已节省
          <span>10</span> )
        </div>
      </div>
      <div class="goBuy">去结算({{count}})</div>
    </div>
    <Footer />
  </div>
</template>

<script>
//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）
//例如：import 《组件名称》 from '《组件路径》';
//引入footer组件
import Footer from "@/components/Footer";
import { mapMutations } from "vuex";
export default {
  //import引入的组件需要注入到对象中才能使用
  components: {
    Footer,
  },
  data() {
    //这里存放数据
    return {};
  },
  //监听属性 类似于data概念
  computed: {
    cartData() {
      return this.$store.state.cartDatas;
    },
    selectAll() {
      let selectStatus = this.$store.state.cartDatas.every((item) => {
        return item.isSelect == true;
      });
      return selectStatus;
    },

    total() {
      let totalPrice = 0;
      this.$store.state.cartDatas.map((item) => {
        if (item.isSelect) {
          totalPrice += item.value * item.shop_price;
        }
      });
      return totalPrice;
    },
    count() {
      let count = 0;
      this.$store.state.cartDatas.map((item) => {
        if (item.isSelect) {
          count += item.value;
        }
      });
      return count;
    },
  },
  //监控data中的数据变化
  watch: {},
  //方法集合
  methods: {
    changeNum() {
      //在组件中执行的方法
      this.$store.commit("changeNum"); //这一行changeNum 是mutations中定义的
    },
    //点击购物车列表数据的选择框
    changSelectStatus(cart, index) {
      console.log(cart);
      cart.isSelect = !cart.isSelect;
      this.$store.commit("changSelectStatus", { cart: cart, index: index });
    },
    // changeSelectAllStatus() {
    //   this.$store.commit("changeSelectAllStatus");
    // },
    ...mapMutations([
      "addCart",
      "jianCart",
      "deleCart",
      "changeSelectAllStatus",
    ]),
    // addCart(index) {
    //   console.log(index);
    //   this.$store.commit("addCart", index);
    // },
    // jianCart(index) {
    //   console.log(index);
    //   this.$store.commit("jianCart", index);
    // },
    // deleCart(index) {
    //   console.log(index);
    //   this.$store.commit("deleCart", index);
    // },
    toDetail(gid) {
      console.log(gid);
      this.$router.push("/goodsdetail/" + gid);
    },
  },
};
</script>
<style lang="less">
.cart-page {
  .cart-footer {
    width: 100%;
    height: 4.4rem;
    position: fixed;
    bottom: 5rem;
    left: 0;
    background: #fff;
    border-top: 1px solid #efefef;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 1.4rem;
    .selectAll {
      width: 20%;
      text-align: center;
    }
    .totalPrice {
      width: 50%;
      text-align: right;
      span {
        &::before {
          content: "￥";
          font-size: 1.2rem;
        }
      }
      .price {
        span {
          font-size: 1.8rem;
          color: #f44;
          font-weight: bold;
        }
      }
      .tips {
        font-size: 1.2rem;
        color: #999;
      }
    }
    .goBuy {
      width: 25%;
      margin-left: 5%;
      background: #f44;
      color: #fff;
      text-align: center;
      line-height: 4.4rem;
    }
  }
  .cart-header {
    width: 100%;
    height: 4.4rem;
    position: fixed;
    top: 0;
    left: 0;
    text-align: center;
    line-height: 4.4rem;
    border-bottom: 1px solid #efefef;
    background: #fff;
    .title {
      font-size: 1.8rem;
    }
  }
  .cart-wrap {
    .cart-list {
      width: 100%;
      ul {
        li {
          height: 10rem;
          border-bottom: 1px solid #efefef;
          display: flex;
          justify-content: space-between;
          align-items: center;
          .select {
            width: 10%;
            text-align: center;
          }
          .cart-content {
            width: 90%;

            display: flex;
            justify-content: space-between;
            align-items: center;
            .cart-img {
              width: 7rem;
              height: 7rem;
              img {
                width: 7rem;
                height: 7rem;
              }
            }
            .cart-text {
              width: 100%;
              width: calc(100% - 9rem);
              margin-right: 1rem;
              display: flex;
              flex-direction: column;
              justify-content: center;
              align-items: center;
              .cart-title {
                font-size: 1.2rem;
                height: 4rem;
                line-height: 2rem;
                overflow: hidden;
                display: -webkit-box;
                -webkit-box-orient: vertical;
                -webkit-line-clamp: 2;
                margin-bottom: 0.5rem;
              }
              .cart-text-wrap {
                width: 100%;
                display: flex;
                justify-content: space-between;
                .price {
                  font-size: 1.8rem;
                  color: #f44;
                  &::before {
                    content: "￥";
                  }
                }
                .num-wrap {
                  display: flex;
                  .icon-shoucang1 {
                    margin: 0 1rem;
                  }
                  .num {
                    display: flex;
                    input {
                      width: 4rem;
                      height: 2rem;
                      border: 1px solid #efefef;
                      text-align: center;
                    }
                    a {
                      display: block;
                      width: 2rem;
                      height: 2rem;
                      background: #efefef;
                      text-align: center;
                      line-height: 2rem;
                      border: 1px solid #efefef;
                    }
                  }
                }
              }
            }
          }
        }
      }
    }
    margin-top: 4.4rem;
    .nocart {
      text-align: center;
      color: #666;
      font-size: 1.4rem;
      margin-top: 10rem;
      i {
        font-size: 15rem;
        color: #efefef;
      }
      div {
        margin-top: 2rem;
      }
      a {
        color: red;
      }
    }
  }
}
</style>