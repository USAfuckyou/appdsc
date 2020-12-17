<!--  -->
<template>
  <div class>
    <h2>vuex状态管理demo---{{statedata}}</h2>
    <div>
      <button @click="changeNum">修改state中的num++</button>
    </div>
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
    return {};
  },
  computed: {
    statedata() {
      return this.$store.state.num;
    },
  },
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