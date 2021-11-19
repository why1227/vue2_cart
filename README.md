# vue2_cart
vue2 购物车案例
一、 初始化项目基本结构
    npm install axios

二、 封装 MyHeader 组件
    1.导入：import Header from "@/components/Header/Header.vue";
    2.注册：components: { Header },
    3.使用：<Header></Header>

三、 基于 axios 请求商品列表数据（ GET 请求，地址为 https://www.escook.cn/api/cart ）
    1.在App中导入axios
    2.在methods方法中，定义initCartList函数请求列表数据
    3.在created生命周期函数中，调用initCartList函数

四、 封装 MyFooter 组件
    计算属性computed，结合父传子

五、 封装 MyGoods 组件
    1.在子组件中，要监听复选框状态变化的事件，拿到最新的勾选状态。
    <input type="checkbox" @change="stateChange"/>
    只要复选框的勾选状态发生了变化，会自动触发change事件。
    2.当监听到勾选状态变化之后，应该立即把最新的状态，通过自定义事件的形式，发送给父组件。
    this.%emit('state-change',{id,value})
    其中：id表示当前这件商品的id，value的值是最新的勾选状态。
    3.父组件：<Goods @click="getNewState"></Goods>
    methods:{
        getNewState(e){
            // 形参中的e就是子组件通过$emit()传递到父组件中的数据，格式为{id,value}
        }
    }

六、 封装 MyCounter 组件
    父传子 + eventBus