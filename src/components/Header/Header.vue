<template>
    <header>
        <!-- 商家基本信息 -->
        <div class="seller">
            <img :src="seller.avatar" alt="">
            <div class="seller-wrapper">
                <h2><i></i>{{seller.name}}</h2>
                <p class="delivery">{{seller.description}}/{{seller.deliveryTime}}分钟到达</p>
                <p class="pay">{{seller.supports[0].description}}</p>
            </div>
            <div class="sw" @click="show=true">
                {{seller.supports.length}}个 >
            </div>
        </div>
        <!-- 公告信息 -->
        <div class="bulletin" @click="show=true">
            <i></i>
            <p>{{seller.bulletin}}</p>
            <span>></span>
        </div>
        <!-- 背景层 -->
        <div class="bg">
            <img :src="seller.avatar" alt="">
        </div>
        <!-- 弹出层 -->
        <transition name="bounce">
            <ele-popup :seller="seller" v-show="show" @close="show=false"></ele-popup>
        </transition>

    </header>
</template>

<script>
import Popup from '../popup/Popup'

export default {
    data(){
        return {
            show : false , //弹出层组件的状态，默认是隐藏
        }
    },
    props: {
        seller : {
            type : Object,
            required : true
        }
    },
    components : {
        'ele-popup' : Popup
    }
}
</script>

<style lang="less" scoped>
    @keyframes bounce-in{
        0%{transform: scale(0);}
        50%{transform: scale(1.5);}
        100%{transform: scale(1);}
    }
    @keyframes bounce-out{
        0%{transform: scale(1);}
        50%{transform: scale(1.5);}
        100%{transform: scale(0);}
    }
    .bounce-enter-active {animation: bounce-in .4s;}
    .bounce-leave-active{animation: bounce-out .4s;}

    header {
        height: 134px;
        width: 100%;
        background: rgba(7,17,27,0.5);
        overflow: hidden;
        position: relative;
        .seller {
            margin : 24px 12px 18px 24px;
            display: flex;
            color:#fff;
            img {
                width: 64px;
                height: 64px;
                border-radius: 4px;
            }
            .seller-wrapper {
                flex-grow: 1;
                margin-left : 16px;
                display: flex;
                flex-direction: column;
                justify-content: space-between;

                h2 {
                    font-size:16px;
                    font-weight: bold;
                    i {
                        display: inline-block;
                        width: 30px;
                        height: 18px;
                        background: url('./brand@2x.png');
                        background-size: contain;
                        vertical-align: middle;
                        margin-right: 7px;
                    }
                }
                .delivery {font-size: 12px;}
                .pay {font-size:10px;}
            }
            .sw {
                align-self: flex-end;
                background: rgba(0,0,0,0.2);
                padding: 10px;
                border-radius: 10px;
            }
        }
        .bg {
            width: 100%;
            height: 100%;
            position: absolute;
            top:0;
            left: 0;
            z-index: -1;
            filter : blur(10px);
            img {
                width : 100%;
                height: 100%;
            }
        }
        .bulletin{
            height: 28px;
            line-height: 28px;
            padding: 0 10px;
            background: rgba(7,17,27,0.2);
            color : #fff;
            font-size: 10px;
            display: flex;
            align-items: center;
            i {
                // display: inline-block;
                min-width: 22px;
                min-height: 12px;
                background: url('./bulletin@2x.png') 0 0 no-repeat;
                background-size: contain;
            }
            p {
                white-space: nowrap;
                overflow: hidden;
                text-overflow: ellipsis;
                margin:0 8px;
            }
        }
    }
</style>
