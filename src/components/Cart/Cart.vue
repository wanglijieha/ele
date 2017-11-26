<template>
    <div>
        <!-- 购物车的整体信息 -->
        <div class="cart" @click="toggleList">
            <div class="cart-left">
                <div class="cart-outer">
                    <div class="cart-inner" :class="{active : count}">
                        <i class="iconfont icon-gouwuche"></i>
                    </div>
                    <div class="count" v-show="count">{{count}}</div>
                </div>
            </div>
            <div class="cart-md">
                <strong>￥{{total}}</strong>
                <span>另需配送费￥{{deliveryPrice}}元</span>
            </div>
            <div class="cart-right" :class="{active : total >= minPrice }">
                {{payDesc}}
            </div>
        </div>

        <!-- 购物车列表 -->
        <transition name="slide">
            <div class="cart-list" v-show="showList">
                <div class="list-hd">
                    <h2>购物车</h2>
                    <span @click="clearCart">清空</span>
                </div>
                <ul>
                    <li v-for="food in cartGoods">
                        <span>{{food.name}}</span>
                        <strong>￥{{food.price * food.count}}</strong>
                        <ele-buy :food="food"></ele-buy>
                    </li>
                </ul>
            </div>
        </transition>

        <!-- 遮罩层 -->
        <transition name="fade">
            <div class="mask" v-show="showList"></div>
        </transition>
    </div>

</template>

<script>
import '../../assets/css/font.css'
import Buy from '../Buy/Buy'

export default {
    data(){
        return {
            show : false, //表示购物车列表是否显示
        }
    },
    props : {
        cartGoods : Array,
        minPrice : Number,
        deliveryPrice: Number
    },
    computed : {
        //获取总的商品数量
        count(){
            let cnt = 0;
            this.cartGoods.forEach( item => {
                cnt += item.count;
            });
            return cnt;
        },
        //获取总的价格
        total(){
            let sum = 0;
            this.cartGoods.forEach( item => {
                sum += item.count * item.price;
            });
            return sum;
        },
        //获取结算情况
        payDesc(){
            //需要根据总价格和起送费用来进行判断
            if (this.total == 0) {
                return `￥${this.minPrice}元起送`;
            }
            if (this.total < this.minPrice) {
                let dis = this.minPrice - this.total;
                return `还差￥${dis}元起送`;
            }
            return '去结算';
        },
        //是否显示购物车列表
        showList(){
            // return this.show && this.cartGoods.length;
            if (this.cartGoods.length==0) {
                //如果清空了，就应该将show变成false
                this.show = false;
            }
            return this.show;
        }
    },
    components : {
        'ele-buy' : Buy
    },
    methods : {
        //切换购物车列表的状态
        toggleList(){
            if (this.cartGoods.length) {
                this.show = !this.show;
            }
        },
        //清空购物车
        clearCart(){
            this.cartGoods.forEach( food => food.count = 0);
        }
    }
    // mounted(){
    //     console.log(this.cartGoods);
    // }

}
</script>

<style lang="less" scoped>
    .slide-enter,.slide-leave-active{transform: translateY(100%);}
    .slide-enter-active,.slide-leave-active{transition: all 0.5s;}
    .fade-enter,.fade-leave-active {opacity: 0;}
    .slide-enter-active,.slide-leave-active{transition: all 0.5s;}
    .cart{
        position: fixed;
        z-index: 200;
        width: 100%;
        height: 48px;
        line-height: 48px;
        left : 0;
        bottom: 0;
        background: #141d27;
        color : rgba(255,255,255,.4);
        display: flex;

        .cart-left{
            width: 80px;
            min-width: 80px;
            position: relative;
            .cart-outer{
                width: 56px;
                height: 56px;
                border-radius: 50%;
                position: absolute;
                left : 12px;
                top:-10px;
                background:#141d27;
                .cart-inner {
                    width:44px;
                    height: 44px;
                    border-radius: 50%;
                    position: absolute;
                    left : 6px;
                    top:6px;
                    background: rgba(255, 255, 255, 0.2);
                    text-align: center;
                    line-height: 44px;
                    i {
                        font-size:24px;
                    }
                    &.active{
                        background: #00a0dc;
                        i {
                            color: #fff;
                        }
                    }
                }
                .count{
                    position: absolute;
                    right : 0;
                    top: 0;
                    width: 20px;
                    height: 20px;
                    border-radius: 50%;
                    background: #f00;
                    color:#fff;
                    text-align: center;
                    line-height: 20px;
                }
            }
        }
        .cart-md{
            flex-grow: 1;
            font-size:16px;
            strong {
                font-weight: 700;
                margin-right:12px;
            }
            span {
                border-left: 1px solid rgba(255,255,255,.1);
                padding-left: 12px;
            }
        }
        .cart-right{
            width: 105px;
            min-width: 105px;
            font-size:12px;
            text-align: center;
            &.active{
                background: #00b43c;
                color: #fff;
            }
        }
    }
    .cart-list{
        position: fixed;
        z-index: 150;
        width: 100%;
        left : 0;
        bottom : 48px;
        background: #fff;
        .list-hd{
            height: 40px;
            line-height: 40px;
            background: #f3f5f7;
            position: relative;
            h2 {
                margin-left: 18px;
                color: rgb(7,17,27);
            }
            span {
                position: absolute;
                top : 0px;
                right : 18px;
                font-size: 12px;
                color : rgb(0,160,220);
            }
        }
        ul {
            margin: 0 18px;
            li {
                line-height: 48px;
                border-bottom: 1px solid rgba(7,17,27,.1);
                position: relative;
                strong{
                    position: absolute;
                    right : 84px;
                    top:0;
                    font-size: 14px;
                    color: rgb(240,20,20);
                    font-weight: 700;
                }
                .buy{
                    position: absolute;
                    right: 0px;
                    top: 10px;
                }
            }
        }
    }
    .mask{
        position: fixed;
        left : 0;
        right: 0;
        top: 0;
        bottom: 48px;
        background:rgba(7,17,27,0.6);
    }
</style>
