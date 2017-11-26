<template>
    <div class="food">
        <div class="back" @click="close"> < </div>
        <img :src="food.image" alt="">
        <div class="info">
            <h2>{{food.name}}</h2>
            <p class="sale">月售{{food.sellCount}}份 好评率{{food.rating}}</p>
            <p class="price">
                <strong><span>￥</span>{{food.price}}</strong>
                <del v-if="food.oldPrice">￥{{food.oldPrice}}</del>
            </p>
        </div>
        <div class="intro">
            <h2>商品介绍</h2>
            <p>{{food.info}}</p>
        </div>
        <div class="comments">
            <h2>商品评价</h2>
        </div>
    </div>
</template>

<script>
export default {
    props : {
        food : {
            type : Object,
            required: true
        }
    },
    methods : {
        close(){
            this.$emit('close');
        }
    }
}
</script>

<style lang="less" scoped>
    .food{
        position: fixed;
        left : 0;
        right: 0;
        top : 0;
        bottom: 48px;
        background: #fff;
        .back {
            position: absolute;
            left : 10px;
            top:10px;
            font-size:32px;
            color: #fff;
        }
        img {
            width:100%;
        }
        .info,.intro,.comments{
            margin:18px;
        }
        .info {
            h2 {
                font-size: 14px;
                color:rgb(7,17,27);
                font-weight: 700;
                line-height: 14px;
            }
            .sale {
                font-size :10px;
                color :rgb(147,153,159);
                margin: 8px 0;
            }
            .price {
                strong {
                    color : rgb(240,20,20);
                    font-size: 14px;
                    font-weight: 700;
                    span {
                        font-size: 10px;
                        font-weight: 100;
                    }
                }
                del {
                    font-size :10px;
                    color :rgb(147,153,159);
                    font-weight: 700;
                }
            }

        }
        .intro,.comments {
            h2 {
                font-size: 14px;
                color:rgb(7,17,27);
                font-weight: 700;
                line-height: 14px;
                margin-bottom: 10px;
            }
        }
        .intro {
            p {
                line-height: 24px;
                font-size: 12px;
                color :rgb(77,85,93);
            }

        }
    }
</style>
