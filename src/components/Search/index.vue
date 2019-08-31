<template>
    <div class="search_body">
        <div class="search_input">
            <div class="search_input_wrapper">
                <i class="iconfont icon-sousuo"></i>
                <input type="text" v-model="message">
            </div>					
        </div>
        <div class="search_result">
            <h3>电影/电视剧/综艺</h3>
            <ul>
                <li v-for="item in moviesList" :key="item.id">
                    <div class="img"><img :src="item.img | setWH('128.180')"></div>
                    <div class="info">
                        <p class="nm"><span>{{item.nm}}</span><span>{{item.sc}}分</span></p>
                        <p class="enm">{{item.enm}}</p>
                        <p>{{item.cat}}</p>
                        <p>{{item.rt}}</p>
                    </div>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
// import { setTimeout } from 'timers';
   export default {
       props:{},
       data(){
           return {
               message : '',
               moviesList : []
           }
       },
       mounted(){
           
      },
      methods:{
          cancelRequest(){
            if(typeof this.source ==='function'){
                this.source('终止请求')
            }
          },
      },
      watch:{
          message(newVal){
              var that = this;
              var cityId = this.$store.state.city.id
              this.cancelRequest();
              this.axios.get('/api/searchList?cityId='+cityId+'&kw=a'+newVal,{
                  cancelToken: new this.axios.CancelToken(function(c) {
                        that.source = c;
                  })
              }).then((res)=>{
               var msg = res.data.msg;
               var movies = res.data.data.movies;
               if(msg && movies){
                  this.moviesList = res.data.data.movies.list;
               }
           }).catch((err) => {
                if (this.axios.isCancel(err)) {
                console.log('Rquest canceled', err.message); //请求如果被取消，这里是返回取消的message
                } else {
                    //handle error
                    console.log(err);
                }
            })
          }
      },
      computed:{},
      components:{},
   }
</script>
<style scoped>
#content .search_body{ 
    flex:1; 
    overflow:auto;
}
.search_body .search_input{ 
    padding: 8px 10px; 
    background-color: #f5f5f5; 
    border-bottom: 1px solid #e5e5e5;
}
.search_body .search_input_wrapper{ 
    padding: 0 10px; 
    border: 1px solid #e6e6e6; 
    border-radius: 5px; 
    background-color: #fff; 
    display: flex; 
    line-height: 20px;
}
.search_body .search_input_wrapper i{
    font-size: 16px; 
    padding: 4px 0;
}
.search_body .search_input_wrapper input{ 
    border: none; 
    font-size: 13px; 
    color: #333; 
    padding: 4px 0; 
    outline: none; 
    margin-left: 5px; 
    width:100%;
}
.search_body .search_result h3{ 
    font-size: 15px; 
    color: #999; 
    padding: 9px 15px; 
    border-bottom: 1px solid #e6e6e6;
}
.search_body .search_result li{ 
    border-bottom:1px #c9c9c9 dashed; 
    padding: 12px 0 12px 15px; 
    box-sizing:border-box; 
    display: flex;
}
.search_body .search_result .img{ 
    width: 60px; 
    float:left; 
}
.search_body .search_result .img img{ 
    width: 100%;
}
.search_body .search_result .info{ 
    float:left; 
    margin-left: 15px; 
    flex:1;
}
.search_body .search_result .info p{
    width: 213px;
    color: #666;
    font-size: 13px;
    margin-top: 2px; 
    
}
.search_body .search_result .info p.nm{
    display: flex;
    margin-top: 0; 
    margin-bottom: 2px;
    width: 271px;
}
.search_body .search_result .info p.enm{
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
}
.search_body .search_result .info p:nth-of-type(1) span:nth-of-type(1){ 
    color: #000;
    max-width: 250px;
    font-weight: 600;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
    font-size: 17px; 
    flex:1; 
}
.search_body .search_result .info p:nth-of-type(1) span:nth-of-type(2){ 
    font-size: 16px; 
    color:#fc7103;
    /* flex: 0 0 26px; */
    padding-left: 5px;
}
</style>