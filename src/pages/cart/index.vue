<template>
    <div class="cart">
        <div class="cart-dom">demo</div>
        <div class="btn" @click="domTest">click me!</div>
        <view :style="{ color: activeColor}">view color</view>
        <div @click="goCounter">go couter</div>
    </div>
</template>
<script>
export default {
    data(){
        return{
            activeColor:"red"
        }
    },
    methods:{
        domTest(e){
            console.log(123);
            console.log(e);
            let nodes=document.querySelector(".cart-dom");
            console.log(nodes);
        },
        goCounter(){
            wx.navigateTo({
                url:"../couter/main"
            })
        }
    }
}
</script>
<style>

</style>

