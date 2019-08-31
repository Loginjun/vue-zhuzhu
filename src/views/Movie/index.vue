<template>
   <div id="main" >
      <Header title="猪猪电影"/>
      <div id="content">
			<div class="movie_menu"> 
				<router-link class="city_name" to="/movie/city" tag="div">
					<span>{{$store.state.city.nm}}</span><i class="iconfont icon-lower-triangle"></i>
                </router-link>
                <div class="hot_swtich">
                    <router-link class="hot_item" to="/movie/nowplaying" tag="div">正在热映</router-link>
                    <router-link class="hot_item" to="/movie/comingsoon" tag="div">即将上映</router-link>
                </div>
                <router-link class="search_entry" to="/movie/search" tag="div">
                    <i class="iconfont icon-sousuo"></i>
                </router-link>
			</div>
            <keep-alive>
                <router-view/>
            </keep-alive>
        </div>
      <TabBar />
      <router-view name="detail"/>
   </div>
</template>

<script>

import Header from '@/components/Header'
import TabBar from '@/components/TabBar'
import { msgbox } from '@/components/JS'
import { setTimeout } from 'timers';

   export default {
        name:'Movie',
        props:{},
        data(){
            return {

            }
        },
        mounted(){
            setTimeout(()=>{
                this.axios.get('/api/getLocation').then((res)=>{
                    var msg = res.data.msg;
                    if(msg === 'ok'){
                        var nm = res.data.data.nm;
                        var id = res.data.data.id;
                        if(this.$store.state.city.id == id){
                            return ;
                        }
                        msgbox({
                            title:'定位',
                            content: nm,
                            cancel:'取消',
                            ok:'切换定位',
                            handleOk(){
                                window.localStorage.setItem('nowNm',nm);
                                window.localStorage.setItem('nowId',id);
                                window.location.reload();
                            }
                        })
                    }
                })
            },2500)
        },
        methods:{
            
        },
        watch:{},
        computed:{},
        components:{
            TabBar,
            Header
        },
   }
</script>
<style scoped>
#content{ 
    flex:1; 
    overflow:auto; 
    margin-bottom: 50px; 
    position: relative; 
    display: flex; 
    flex-direction:column;
}
#content .movie_menu{ 
     width: 100%; height: 45px;
     border-bottom:1px solid #e6e6e6; 
     display: flex; 
     justify-content:space-between; 
     align-items:center; 
     background:white; 
     z-index:10;
}
.movie_menu .hot_swtich .router-link-active{
    color: #ef4238; 
    border-bottom: 2px #ef4238 solid;
}
.movie_menu .city_name{ 
    color: #666;
    margin-left: 15px; 
    height:100%; 
    line-height: 45px;
    font-size: 15px;
    display: flex;
    -webkit-box-align: center;
    align-items: center;
}
.movie_menu .city_name span{
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    max-width: 70px;
    max-width: 19.2vw;
}
.movie_menu .city_name.active{ 
    color: #ef4238; 
    border-bottom: 2px #ef4238 solid;
}
.movie_menu .hot_swtich{ 
    display: flex; 
    height:100%; 
    line-height: 45px;
}
.movie_menu .hot_item{ 
    font-size: 15px; 
    color:#666; 
    width:80px; 
    text-align:center; 
    margin:0 8px; 
    font-weight:700;
}
/* .movie_menu .hot_item:last-child{
    margin-left: 0;
} */
.movie_menu .hot_item.active{ 
    color: #ef4238; 
    border-bottom: 2px #ef4238 solid;
}
.movie_menu .search_entry{ 
    margin-right:15px; 
    height:100%; 
    line-height: 45px;
}
.movie_menu .search_entry.active{ 
    color: #ef4238; 
    border-bottom: 2px #ef4238 solid;
}
.movie_menu .search_entry i{  
    font-size:24px; 
    color:red;
}
</style>