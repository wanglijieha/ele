<template>
    <div class="buy">
        <transition name="rotate">
            <div class="sub" v-show="food.count" @click.stop="deleteFromCart(food)">
                <i class="iconfont icon-jian"></i>
            </div>
        </transition>

        <transition name="fade">
            <p v-show="food.count">{{food.count}}</p>
        </transition>

        <div class="add" @click.stop="addToCart(food)">
            <i class="iconfont icon-jia"></i>
        </div>
    </div>
</template>

<script>
export default {
    props:{
        food : {
            type : Object,
            required : true
        }
    },
    methods : {
        //添加商品到购物车
        addToCart(food){
            //判断food中是否存在count属性，如果没有，就添加，如有，就++
            if (food.count) {
                food.count++;
            } else {
                //添加的没有声明的属性
                this.$set(food,'count',1);
            }
        },
        //从购物车中减少数量或删除
        deleteFromCart(food){
            food.count--;
        }
    }
}
</script>

<style lang="less" scoped>
    .rotate-enter,.rotate-leave-active{transform: translateX(48px) rotate(360deg);}
    .rotate-enter-active, .rotate-leave-active{transition: all 0.5s;}
    .fade-enter,.fade-leave-active{opacity: 0;}
    .fade-enter-active,.fade-leave-active{transition: all 0.5s;}
    .buy {
        width: 72px;
        height: 24px;
        line-height: 24px;
        position: relative;
        .sub {
            position: absolute;
            left: 0;
            top:0;
        }
        .add{
            position: absolute;
            right: 0;
            top:0;
        }
        .sub i,.add i{
            font-size: 24px;
            color :rgb(0,160,220);
        }
        .add i {
            position: relative;
            top:3px;
        }
        p {
            position: absolute;
            left : 50%;
            transform: translateX(-50%);
            top:0;
            font-size: 10px;
            color :rgb(147,153,159);
        }
    }
</style>
