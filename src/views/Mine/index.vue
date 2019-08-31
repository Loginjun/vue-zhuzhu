<template>
   <div id="main" >
      <Header title="我的猪猪"/>
      <div id="content">
         <Login/>
      </div>
      <TabBar />
   </div>
</template>

<script>
import Header from '@/components/Header'
import TabBar from '@/components/TabBar'
import Login from '@/components/Login'

   export default {
       props:{},
       data(){
           return {

           }
       },
      methods:{},
      watch:{},
      computed:{},
      components:{
          TabBar,
          Header,
          Login
      },
   }
</script>
<style scoped>

</style>