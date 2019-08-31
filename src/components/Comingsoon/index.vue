<template>
   <div class="movie_body" ref="movie_body">
         <Loading v-if="isLoading"/>
         <ul>
            <li v-for="item in comingList" :key="item.id">
               <div class="pic_show" @tap="handleTo(item.id)"><img :src="item.img | setWH('128.180')"></div>
               <div class="info_list" @tap="handleTo(item.id)">
                  <h2>{{item.nm}} <img v-if="item.version" src="@/assets/maxs.png" alt=""></h2>
                  <p><span class="person">{{item.wish}}</span> 人想看</p>
                  <p>主演: {{item.star}}</p>
                  <p>{{item.showInfo}}</p>
               </div>
               <div class="btn_pre" v-if="item.showst === 4">预售</div>
               <div class="btn_nex" v-if="item.showst === 1">想看</div>
            </li>
         </ul>
	</div>
</template>

<script>
   import BScroll from 'better-scroll';
   export default {
       props:{},
       data(){
           return {
              comingList:[],
              isLoading:true,
              prevCityId : -1
           }
       },
       activated(){
            var cityId = this.$store.state.city.id;
            if(this.prevCityId === cityId){
               return ;
            }
            this.isLoading = true;
            this.axios.get('/api/movieComingList?cityId='+cityId).then((res)=>{
               var msg = res.data.msg;
               if(msg === 'ok'){
                  this.comingList = res.data.data.comingList;
                  this.isLoading = false;
                  this.prevCityId = cityId;
                  this.$nextTick(()=>{
                     new BScroll(this.$refs.movie_body,{
                        tap:true,
                        // probeType:1
                     });
                  });
               }
           })
      },
      methods:{
         handleTo(movieId){
            this.$router.push('/movie/detail/cs/'+movieId)
         }
      },
      watch:{},
      computed:{},
      components:{},
   }
</script>
<style scoped>
#content .movie_body{ flex:1; overflow:auto;}
.movie_body ul{ margin:0 12px; overflow: hidden;}
.movie_body ul li{ margin-top:12px; display: flex; align-items:center; border-bottom: 1px #e6e6e6 solid; padding-bottom: 10px;}
.movie_body .pic_show{ width:64px; height: 90px;}
.movie_body .pic_show img{ width:100%;}
.movie_body .info_list { margin-left: 10px; flex:1; position: relative;}
.movie_body .info_list h2{ font-size: 17px; line-height: 24px; width:150px; overflow: hidden; white-space: nowrap; text-overflow:ellipsis;}
.movie_body .info_list p{ font-size: 13px; color:#666; line-height: 22px; width:200px; overflow: hidden; white-space: nowrap; text-overflow:ellipsis;}
.movie_body .info_list p .person{
      color: #faaf00;
      font-size: 15px;
      font-weight: 600;
}
.movie_body .info_list .grade{ font-weight: 700; color: #faaf00; font-size: 15px;}
.movie_body .info_list img{
   width:50px;
   position: absolute; 
   right:10px; 
   top: 5px;
}
.movie_body .btn_mall ,
.movie_body .btn_pre,
.movie_body .btn_nex{ 
   width:47px; 
   height:27px; 
   line-height: 28px; 
   text-align: center; 
   background-color: #f03d37; 
   color: #fff; 
   border-radius: 4px; 
   font-size: 12px; 
   cursor: pointer;
}
.movie_body .btn_pre{ 
   background-color: #3c9fe6;
}
.movie_body .btn_nex{
   background-color: #faaf00;
}
</style>