<template>
    <div class="wrapper" ref="wrapper">
        <slot></slot>
    </div>
</template>

<script>
    import BScroll from 'better-scroll';
    export default {
        name:'ScrollView',
        props:{},
        data(){
            return {

            }
        },
        mounted(){
            var scroll = new BScroll(this.$refs.wrapper,{
                tap:true,
                probeType:1
            });
            this.scroll = scroll;
        },
        methods:{
            handleIndexTop(y){
                this.scroll.scrollTo(0,y);
            }
        },
        watch:{},
        computed:{},
        components:{},
    }
</script>
<style scoped>
   .wrapper{
       height: 100%;
   }
</style>