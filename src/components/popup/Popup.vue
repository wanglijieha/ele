<template>
    <div class="popup">
        <h2>{{seller.name}}</h2>
        <h3>优惠信息</h3>
        <ul>
            <li v-for="item in seller.supports">{{item.description}}</li>
        </ul>
        <h3>商家公告</h3>
        <p>{{seller.bulletin}}</p>
        <div class="close" @click="close"> x </div>
    </div>
</template>

<script>
export default {
    props : {
        seller : {
            type : Object,
            required : true
        }
    },
    methods: {
        close(){
            //需要向父组件emit一个事件
            this.$emit('close');
        }
    }
}
</script>

<style lang="less" scoped>
    .popup{
        position: fixed;
        z-index : 999;
        width: 100%;
        height: 100%;
        top: 0;
        left : 0;
        background: rgba(7,17,27,.8);
        color: #fff;
        font-size: 12px;
        h2 {
            margin-top:64px;
            font-size: 16px;
            font-weight: 700;
            text-align: center;
        }
        h3 {
            font-size: 14px;
            font-weight: 700;
            text-align: center;
            margin:15px 0 ;
        }
        ul {
            margin:0 36px;
            li {
                line-height: 24px;
            }
        }
        p {
            margin: 0 36px;
            line-height: 24px;
        }
        .close {
            font-size:32px;
            color : rgba(255,255,255,.5);
            position: absolute;
            bottom: 32px;
            left : 50%;
            transform: translateX(-50%);
        }
    }
</style>
