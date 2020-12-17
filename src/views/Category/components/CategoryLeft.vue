<!-- 分类左侧 -->
<template>
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
  methods: {
    changeTab(cat_id) {
      console.log(cat_id);
      this.cat_id = cat_id;
      this.$store.dispatch("actChangeCategoryRight", cat_id);
    },
  },

  computed: {
    catId() {
      return this.$store.state.cat_id;
    },
  },
  mounted() {
    // catId==categoryLeftData.cat_id
    console.log(this.$store.state.cat_id);
  },
};
</script>
<style lang="less">
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
</style>