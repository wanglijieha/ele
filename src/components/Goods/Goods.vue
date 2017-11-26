<template>
    <div>
        <div class="goods">
            <!-- 左边的分类菜单  -->
            <div class="menu" ref="menu">
                <ul>
                    <li v-for="(item,index) in goods" :class="{active:index==current}"
                    @click="selectFoods(index)">
                        <p>
                            {{item.name}}
                        </p>
                    </li>
                </ul>
            </div>
            <!-- 右边的商品列表 -->
            <div class="goods-list" ref="list">
                <div>
                    <dl v-for="item in goods">
                        <dt>{{item.name}}</dt>
                        <dd v-for="food in item.foods" @click="selectFood(food)">
                            <div class="food">
                                <img :src="food.icon" alt="">
                                <div>
                                    <h3>{{food.name}}</h3>
                                    <p class="desc">{{food.description}}</p>
                                    <p class="sale">月售{{food.sellCount}}份 好评率{{food.rating}}</p>
                                    <p class="price">
                                        <strong><span>￥</span>{{food.price}}</strong>
                                        <del v-if="food.oldPrice">￥{{food.oldPrice}}</del>
                                    </p>
                                </div>
                            </div>
                            <div class="cart">
                                <ele-buy :food="food"></ele-buy>
                            </div>
                        </dd>
                    </dl>
                </div>
            </div>
        </div>
        <transition name="slide">
            <ele-food :food="food" v-show="show" @close="show=false"></ele-food>
        </transition>
        <ele-cart
            :cartGoods="cartGoods"
            :minPrice="seller.minPrice"
            :deliveryPrice="seller.deliveryPrice">
        </ele-cart>
    </div>
</template>

<script>
// import ele from '../../../data.json';
// import IScroll from '../../assets/js/iscroll.js'
require('../../assets/js/iscroll-probe.js');
import Food from '../Food/Food'
import Cart from '../Cart/Cart'
import Buy from '../Buy/Buy'

export default {
    data(){
        return {
            goods : [], //所有的商品数据
            seller :{}, //表示商家的信息
            current : 0 , //表示当前是哪一个分类
            listHeight : [] ,// 表示所有dl的高度
            food : {}, //表示当前的外卖商品
            show : false, //详情组件的显示和隐藏
        }
    },
    computed: {
        //获取购物车中的商品
        cartGoods(){
            // return this.goods.filter( item => item.count );
            let arr = [];
            this.goods.forEach(item1 => {
                item1.foods.forEach( item2 => {
                    if (item2.count) {
                        arr.push(item2);
                    }
                });
            });
            return arr;
        }
    },
    components : {
        'ele-food' : Food,
        'ele-cart' : Cart,
        'ele-buy' : Buy
    },
    methods : {
        //点击左侧的分类
        selectFoods(index){
            this.current = index;
            //获取所有的dl，得到一个类数组，可以通过dls[index]获取应该显示的dl
            let dls = this.$refs.list.getElementsByTagName('dl');
            this.listScroll.scrollToElement(dls[index]);
        },
        //点击商品
        selectFood(food){
            this.food = food;
            //需要设置food的显示
            this.show = true;
        },
        //定义一个方法，用于获取dl的高度
        _getListHeight(){
            let arr = [0];
            let dls = this.$refs.list.getElementsByTagName('dl');
            for(let i = 0; i < dls.length; i++) {
                arr.push( arr[i] + dls[i].clientHeight );
            }
            return arr;
        }
    },
    created(){
        let ele = require('../../../data.json'); //此处不能使用import
        this.goods = ele.goods;
        this.seller = ele.seller;
    },
    mounted(){
        this.listScroll = new IScroll(".goods-list",{probeType:2});
        this.menuScroll = new IScroll(".menu");
        //注册scroll事件
        let _this = this;
        this.listScroll.on('scroll',function(){
            // console.log(this.y);
            let dis = Math.abs(this.y);
            // console.log(dis);
            //需要根据dis 和 listHeight进行判断，看对应的是第几个分类
            for (let i = 0; i < _this.listHeight.length; i++ ) {
                let start = _this.listHeight[i];
                let end = _this.listHeight[i+1];
                if (!end ||  (start <= dis && dis < end) ) {
                    _this.current = i;
                    break;
                }
            }
            //需要滚动
            let lis = _this.$refs.menu.getElementsByTagName("li");
            _this.menuScroll.scrollToElement(lis[_this.current]);
        });
        //调用getListHeight方法
        this.listHeight = this._getListHeight();
        // console.log(this.listHeight);
    }
}
</script>

<style lang="less" scoped>
    .slide-enter, .slide-leave-active{opacity: 0; transform:translateX(100%); }
    .slide-enter-active, .slide-leave-active {transition : all 0.5s;}
    .goods {
        position: fixed;
        left :0;
        right : 0;
        top:175px;
        bottom: 48px;
        overflow: hidden;
        display: flex;
        .menu{
            min-width: 80px;
            width:80px;
            background: #f3f5f7;
            li {
                height: 54px;
                padding: 0 12px;
                color : rgb(77,85,93);
                font-size:12px;
                &:last-child{
                    border-bottom:none;
                }
                &.active {
                    background: #fff;
                    p {
                        border: none;
                    }
                }
                p {
                    display: flex;
                    align-items: center;
                    height: 100%;
                    border-bottom: 1px solid rgba(7,17,27,0.1);
                }
            }
        }
        .goods-list{
            flex-grow: 1;
            dt {
                height: 26x;
                line-height: 26px;
                font-size: 12px;
                color : rgb(147,153,159);
                background: #f3f5f7;
                border-left: 3px solid #d9dde1;
                padding-left: 14px;
            }
            dd {
                position: relative;
                margin:18px;
                .food {
                    display: flex;
                    img {
                        width: 57px;
                        height: 57px;
                        margin-right: 10px;
                    }
                    h3 {
                        font-size: 14px;
                        color : rgb(7,17,27);
                    }
                    .desc,.sale {
                        font-size:10px;
                        color : rgb(147,153,159);
                        margin : 8px 0;
                    }
                    .price {
                        strong {
                            color : rgb(240,20,20);
                            font-size : 14px;
                            font-weight: bold;
                            span {font-size: 10px};
                        }
                        del{
                            font-size:10px;
                            font-weight: bold;
                            color:rgb(147,153,159);
                        }
                    }
                }
                .cart{
                    position: absolute;
                    right : 0;
                    bottom: 0;
                }
            }
        }
    }
</style>
