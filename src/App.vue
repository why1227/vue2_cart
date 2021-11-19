<template>
  <div class="app-container">
    <Header title_name="why购物车"></Header>

    <!-- 循环渲染每一个商品的信息 -->
    <Goods
      v-for="item in list"
      :key="item.id"
      :title="item.goods_name"
      :pic="item.goods_img"
      :price="item.goods_price"
      :state="item.goods_state"
      :count="item.goods_count"
      :id="item.id"
      @state-change="getNewState"
    ></Goods>
    <Footer
      :isFull="fullState"
      :amount="amt"
      :all="total"
      @full-change="getFullState"
    ></Footer>
  </div>
</template>

<script>
// 导入库
import axios from "axios";
// 导入需要的组件
import Header from "@/components/Header/Header.vue";
import Goods from "@/components/Goods/Goods.vue";
import Footer from "@/components/Footer/Footer.vue";
import bus from "@/components/eventBus.js";
export default {
  data() {
    return {
      // 用来存储购物车的列表数据，默认空数组
      list: [],
    };
  },
  components: {
    Header,
    Goods,
    Footer,
  },
  methods: {
    // 封装请求列表数据的方法
    async initCartList() {
      const { data: res } = await axios.get("https://www.escook.cn/api/cart");
      // 只要请求回来的数据，在页面渲染期间要用到，则必须转存到data里
      if (res.status === 200) {
        this.list = res.list;
      }
    },
    // 接收子组件传递过来的数据
    getNewState(e) {
      // e的格式为：{id: 4, value: true}
      this.list.some((item) => {
        if (item.id == e.id) {
          item.goods_state = e.value;
          return true;
        }
      });
    },
    // 接受footer子组件传递过来的全选按钮的状态
    getFullState(val) {
      this.list.forEach((item) => (item.goods_state = val));
    },
  },
  created() {
    this.initCartList();
    bus.$on("change-count", (val) => {
      this.list.some((item) => {
        if (item.id === val.id) {
          item.goods_count = val.value;
          return true;
        }
      });
    });
  },
  computed: {
    // 动态计算出全选的状态
    fullState() {
      // return this.list.every((item) => item.goods_state === true);
      return this.list.every((item) => item.goods_state);
    },
    // 已勾选商品的总价
    amt() {
      return this.list
        .filter((item) => item.goods_state)
        .reduce(
          (total, item) => (total += item.goods_count * item.goods_price),
          0
        );
    },
    // 计算已勾选商品总数量
    total() {
      return this.list
        .filter((item) => item.goods_state)
        .reduce((t, item) => (t += item.goods_count), 0);
    },
  },
};
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
