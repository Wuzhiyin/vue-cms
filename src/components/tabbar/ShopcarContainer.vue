<template>
    <div class="shopcar-container">
        <div class="goods-list">
            <!-- 商品列表项区域 -->
            <div class="mui-card" v-for="item in goodslist" :key="item.id">
                <div class="mui-card-content">
                    <div class="mui-card-content-inner">
                        <mt-switch></mt-switch>
                        <img :src="item.thumb_path">
                        <div class="info">
                            <h1>{{ item.title }}</h1>
                            <p>
                                <span class="price">￥{{ item.sell_price }}</span>
                                <numbox :initcount="$store.getters.getGoodsCount[item.id]"></numbox>
                                <a href="#">删除</a>
                            </p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <!-- 结算区域 -->
        <div class="mui-card">
            <div class="mui-card-content">
                <div class="mui-card-content-inner">
                    TODO
                </div>
            </div>
        </div>
        <p>{{ $store.getters.getGoodsSelected }}</p>
    </div>
</template>
<script>
import numbox from "../subcomponents/shopcar_numbox.vue";

export default {
    data() {
            return {
                goodslist: [] // 购物车中所有商品的数据
            };
        },
        created() {
            this.getGoodsList();
        },
        methods: {
            getGoodsList() {
                // 1. 获取到 store 中所有的商品的Id，然后拼接出一个 用逗号分隔的 字符串
                var idArr = [];
                this.$store.state.car.forEach(item => idArr.push(item.id));
                // 如果 购物车中没有商品，则直接返回，不需要请求数据接口，否则会报错
                if (idArr.length <= 0) {
                    return;
                }
                // 获取购物车商品列表
                this.$http
                    .get("api/goods/getshopcarlist/" + idArr.join(","))
                    .then(result => {
                        if (result.body.status === 0) {
                            this.goodslist = result.body.message;
                        }
                    });
            },
        },
        components: {
            numbox
        }
};
</script>
<style lang="scss" scoped>
.shopcar-container {
    background-color: #eee;
    overflow: hidden;
    .goods-list {
        .mui-card-content-inner {
            display: flex;
            align-items: center;
        }
        img {
            width: 60px;
            height: 60px;
        }
        h1 {
            font-size: 13px;
        }
        .info {
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            .price {
                color: red;
                font-weight: bold;
            }
        }
    }
}
</style>
