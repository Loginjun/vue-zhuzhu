<template>
    <div class="msg">
        <h2>{{title}}</h2>
        <p>{{content}}</p>
        <div>
            <div @touchstart="handleCancel">{{cancel}}</div>
            <div @touchstart="handleOk">{{ok}}</div>
        </div>
    </div>
</template>

<script>
    export default {
        name:'MsgBox',
        props:{},
        data(){
            return {

            }
        },
        methods:{},
        watch:{},
        computed:{},
        components:{},
    }
</script>
<style scoped>
    .msg{
        width: 200px;
        height: 120px;
        border: 1px solid #ccc;
        border-radius: 4px;
        background: white;
        box-shadow: 1px 1px 1px 1px #ccc;
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translateX(-50%) translateY(-50%);
    }
    .msg h2{
        text-align: center;
        line-height: 40px;
        font-size: 18px;
    }
    .msg p{
        text-align: center;
        line-height: 40px;
    }
    .msg >div{
        display: flex;
        position: absolute;
        bottom: 0;
        width: 100%;
    }
    .msg >div div{
        flex: 1;
        text-align: center;
        line-height: 30px;
        border-right: 1px solid #ccc;
    }
    .msg >div div:last-child{
        border-right:none;
    }
</style>