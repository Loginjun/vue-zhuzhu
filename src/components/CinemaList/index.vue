<template>
   <div class="cinema_body">
        <Loading v-if="isLoading"/>
       <ScrollView v-else>
        <ul>
            <li v-for="item in cinemaList" :key="item.id">
                <div>
                    <span>{{item.nm}}</span>
                    <span class="q"><span class="price"> {{item.sellPrice}} </span> 元起</span>
                </div>
                <div class="address">
                    <span class="addr">{{item.addr}}</span>
                    <span>{{item.distance}}</span>
                </div>
                <div class="card">
                    <div v-for="(itemCard,key) in item.tag" :key="key" v-if="itemCard === 1" :class="key | classCard">{{key | formatCard}}</div>
                    <div v-if="item.tag.vipTag != null ">{{item.tag.vipTag}}</div>
                    <div class="bl" v-for="(i,index) in item.tag.hallTypeVOList" :key="index">{{i.name}}</div>
                </div>
                <div class="discounts">
                    <div class="bl" v-for="(k,index) in item.promotion" :key="index">{{k}}</div>
                </div>
            </li>
        </ul>
        </ScrollView>
	</div>
</template>

<script>
   export default {
        name:'CinemaList',
        props:{},
        data(){
            return {
                cinemaList:[],
                isLoading:true,
                prevCityId: -1
            }
        },
        activated(){
            var cityId = this.$store.state.city.id;
            if(this.prevCityId === cityId){
               return ;
            }
            this.isLoading = true;
            this.axios.get('/api/cinemaList?cityId='+cityId).then((res)=>{
                var msg = res.data.msg;
                if(msg === 'ok'){
                    this.cinemaList = res.data.data.cinemas;
                    this.isLoading = false
                    this.prevCityId = cityId
                }
            })
        },
        filters:{
            formatCard(key){
                var card = [
                    {key:'endorse',value:'改签'},
                    {key:'allowRefund',value:'退'},
                    {key:'sell',value:'售'},
                    {key:'snack',value:'小吃'},
                    {key:'hallType',value:'60帧厅'},
                    {key:'hallTypeVOList',value:'IMAX厅'}
                ];
                for(var i = 0;i<card.length;i++){
                    if(card[i].key == key){
                        return card[i].value;
                    }
                }
                return '';
            },
            classCard(key){
                var card = [
                    {key:'allowRefund',value:'bl'},
                    {key:'endorse',value:'bl'},
                    {key:'sell',value:'or'},
                    {key:'snack',value:'or'}
                ];
                for(var i = 0;i<card.length;i++){
                    if(card[i].key == key){
                        return card[i].value;
                    }
                }
                return '';
            }
        },
        methods:{},
        watch:{},
        computed:{},
        components:{},
        
    }
</script>
<style scoped>
#content .cinema_body{ 
    flex:1; 
    overflow:auto;
}
.cinema_body ul{ 
    padding: 0 20px;
}
.cinema_body li{  
    border-bottom:1px solid #e6e6e6; 
    /* margin-bottom: 20px; */
    padding: 13px 15px 13px 0;
}
.cinema_body div{ 
    /* margin-bottom: 10px; */
}
.cinema_body .q{ 
    font-size: 11px; 
    color:#f03d37;
}
.cinema_body .price{ 
    font-size: 18px;
}
.cinema_body .address{ 
    margin-top: 6px;
    font-size: 13px; 
    color:#666;
}
.cinema_body .address .addr{
    display: inline-block;
    width: 250px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
}
.cinema_body .address span:nth-of-type(2){ 
    float:right; 
}
.cinema_body .card{ 
    height: 17px;
    line-height: 17px;
    margin-top: 4px;
    margin-bottom: 4px;
    overflow: hidden;
    font-size: 0;
    flex-shrink: 0;
}
.cinema_body .card div{ 
    display: inline-block;
    padding: 0 3px; 
    height: 15px; 
    line-height: 15px; 
    border-radius: 2px; 
    color: #f90; 
    border: 1px solid #f90; 
    font-size: .6rem; 
    margin-right: 5px;
}
.cinema_body .card div.or{ 
    color: #f90; 
    border: 1px solid #f90;
}
.cinema_body .card div.bl{ 
    color: #589daf; 
    border: 1px solid #589daf;
}
.cinema_body .discounts{
    margin-left: 0;
    font-size: 11px;
    margin-bottom: 4px;
    color: #999;
}
</style>