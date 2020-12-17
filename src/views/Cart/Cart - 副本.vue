<!--  -->
<template>
  <div class>
    <h2>购物车--{{this.$store.state.num}}</h2>
    <button @click="changeNum">修改state中的num++</button>
    <Footer />
  </div>
</template>

<script>
//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）
//例如：import 《组件名称》 from '《组件路径》';
//引入footer组件
import Footer from "@/components/Footer";
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
  computed: {},
  //监控data中的数据变化
  watch: {},
  //方法集合
  methods: {
    changeNum() {
      //在组件中执行的方法
      this.$store.commit("changeNum"); //这一行changeNum 是mutations中定义的
    },
  },
};
</script>
<style>
</style>