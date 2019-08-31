<template>
    <div id="detailContainer" class="move-active">
        <Header :title="detailMovie.nm">
            <i class="iconfont icon-right" @touchstart="toBack"></i>
        </Header>
        <Loading v-if="isloading"/>
        <div v-else id="content" class="contentDetail">
            <div class="detail_list">
                <div class="detail_list_bg" :style="{
                    'background':'url('+detailMovie.img.replace(/w\.h/,'148.208')+')no-repeat',
                    'background-size': 'cover'
                }"></div>
                <div class="detail_list_filter"></div>
                    <div class="detail_list_content">
                        <div class="detail_list_img">
                            <img v-if="detailMovie.img" :src="detailMovie.img | setWH('148.208')" />
                        </div>
                        <div class="detail_list_info">
                            <h2>{{detailMovie.nm}}</h2>
                            <div class="title-name">{{detailMovie.enm}}</div>
                            <div><span class="score">{{detailMovie.sc}}</span> 分</div>
                            <div>{{detailMovie.cat}}</div>
                            <div>{{detailMovie.src}} / {{detailMovie.dur}}分钟</div>
                            <div>{{detailMovie.pubDesc}}</div>
                        </div>
                    </div>
            </div>
      <div class="detail_intro">
          <a href="#" class="btn"><span>特惠购票</span></a>
        <p>{{detailMovie.dra}}</p>
      </div>
      <WhiteSpace/>
      <div class="detail_player swiper-container" ref="detail_player">
            <h3>媒体库</h3>
            <ul class="swiper-wrapper">
                <li class="swiper-slide" v-for="(ph,index) in detailMovie.photos" :key="index">
                        <div>
                            <img :src="ph | setWH('180.140')" alt />
                        </div>
                        <!-- <p>陈建斌</p>
                        <p>马先勇</p> -->
                </li>
            </ul>
            <h4>剧照<span></span></h4>
      </div>
    </div>
  </div>
</template>

<script>
import WhiteSpace from '@/components/WhiteSpace'
import Header from '@/components/Header'
import { setTimeout } from 'timers';

export default {
    name:'detail',
    props: ['movieId'],
    data() {
        return {
            detailMovie:[],
            isloading:true
        };
    },
    methods: {
        toBack(){
            this.$router.go(-1);
        }
    },
    mounted(){
        console.log(this.movieId)
         this.axios.get('/api/detailmovie?movieId='+this.movieId).then((res)=>{
            var msg = res.data.msg
            if(msg === 'ok'){
               this.detailMovie = res.data.data.detailMovie;
                this.isloading = false;
               this.$nextTick(()=>{
                   new Swiper(this.$refs.detail_player,{
                       slidesPerView:'auto',
                       freeMode:true,
                       freeModeSticky:true
                   })
               })
            }
         })
      },
    watch: {},
    computed: {},
    components: {
        Header,
        WhiteSpace
    }
};
</script>
<style scoped>
#detailContainer {
    position: absolute;
    top: 0;
    left: 0;
    z-index: 100;
    width: 100%;
    min-height: 100%;
    background: #fff;
}
#detailContainer.move-active{
    animation: .5s move;
}
@keyframes move {
    0%{
        transform: translateX(100%)
    }
    100%{
        transform: translateX(0)
    }
}
#content .detail_list {
    height: 188px;
    width: 100%;
    position: relative;
    overflow: hidden;
}
.detail_list .detail_list_bg {
    width: 100%;
    height: 100%;
    filter: blur(20px);
    position: absolute;
    left: 0;
    top: 0;
}
.detail_list .detail_list_filter {
    width: 100%;
    height: 100%;
    position: absolute;
    background-color: #40454d;
    opacity: 0.55;
    position: absolute;
    left: 0;
    top: 0;
    z-index: 1;
}
.detail_list .detail_list_content {
    display: flex;
    width: 100%;
    height: 100%;
    position: absolute;
    left: 0;
    top: 0;
    z-index: 2;
    padding: 19px 30px 19px 15px;
    box-sizing: border-box;
}
.detail_list .detail_list_img {
    width: 110px;
    height: 150px;
    cursor: pointer;
}
.detail_list .detail_list_img img {
    width: 100%;
    height: 100%;
}
.detail_list .detail_list_info {
    margin-left: 12.5px; 
}
.detail_list .detail_list_info h2 {
    font-size: 20px;
    font-weight: 700;
    color: white;
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
}
.detail_list .detail_list_info div{
    margin-top: 7px;
    font-size: 12px;
    color: #fff;
    opacity: .8;
}

.detail_list .detail_list_info .score{
    font-size: 18px;
    font-weight: 700;
    color: #fc0;
}

#content .detail_intro {
    padding: 15px;
}
#content .detail_intro .btn{
    text-decoration: none;
    display: block;
    padding: 10px;
    line-height: 1;
    border-radius: 4px;
    background: #e54847;
    cursor: pointer;
    text-align: center;
    margin-bottom: 10px;
    -webkit-tap-highlight-color:rgba(0,0,0,0);
}

#content .detail_intro .btn span{
    display: inline-block;
    font-size: 16px;
    color: white;
}
#content .detail_player {
    margin: 0 0;
}
#content .detail_player h3{
    padding: 10px 15px;
    margin: 0;
    color: #666;
    font-size: 15px;
}
#content .detail_player h4{
    padding: 5px 15px;
    margin: 0;
    color: #666;
    font-size: 14px;
    font-weight: lighter;
    line-height: 40px;
    padding: 0 15px;
}
.detail_player .swiper-wrapper{
    box-sizing: border-box;
    padding-left: 15px; 
}
.detail_player .swiper-slide {
    margin-right: 5px;
    width: 92px;
    height: 72px;
    text-align: center;
    font-size: 14px;
}
.detail_player .swiper-slide:last-child{
    margin-right: 30px;
}
.detail_player .swiper-slide div{
    width: 100%;
    height: 100%;
}
.detail_player .swiper-slide img {
    display: inline-block;
    width: 100%;
    height: 100%;
    margin-bottom: 5px;
}
.detail_player .swiper-slide p:nth-of-type(2) {
    color: #999;
}
</style>