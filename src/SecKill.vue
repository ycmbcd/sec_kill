<template>
    <div style="background:#fffcdb">
        <home-banner :list="banner_1"></home-banner>
        <div class="sec_item" v-for="item of sec_kill" :key="item.id">
            <div v-if="item.status=='close'" class="close_item">
                <div class="end_txt">終了しました！</div>
            </div>
            <div v-if="item.status=='ready'" class="ready_item">
                <div class="end_txt">Ready Start！</div>
            </div>
            <div class="center p_time">{{ item.date }}<br>
            <span style="font-size:18px;">{{ item.start_time }}～{{ item.end_time }}</span></div>
            <a :href="item.url">
                <img :src="item.img">
                <div class="p_txt">{{ item.txt }}{{today}}</div>
            </a>
            <div class="line"></div>
            <div class="p_box">
                <div class="f_left price">{{ item.price }}</div>
                <div class="f_right o_price">{{ item.o_price }}</div>
                <div class="clear"></div>
            </div>
            <div style="height:.2rem"></div>
            <div class="center">
                <span v-if="item.status!='start'" class="p_panel">{{ item.dd }}</span>
                <span v-if="item.status!='start'"> 日 </span>
                <span class="p_panel">{{ item.hh }}</span> :
                <span class="p_panel">{{ item.mm }}</span> :
                <span class="p_panel">{{ item.ss }}</span>
                <span v-if="item.status=='start'"> : </span>
                <span class="p_panel" v-if="item.status=='start'">{{ item.xxx }}</span> 
            </div>
        </div>
        <div class="clear"></div>
        <div style="height:1rem"></div>
        <home-banner :list="banner_2"></home-banner>
    </div>
</template>

<script>
import axios from "axios";
import moment,{ now } from "moment";
import HomeBanner from "./HomeBanner";

    export default {
        name: 'HomeBox',
        components: {
            HomeBanner
        },
        data(){
            return {
                sec_kill:[],
                today: '',
                banner_1: [],
                banner_2: []
            }
        },
        methods: { 
            zeroTwo(num) {
                if(num < 10) {
                    return "0" + num.toString();
                }
                if(num < 100) {
                    return num.toString();
                }
            },
            zeroThree(num) {
                if(num < 10) {
                    return "00" + num.toString();
                }
                if(num < 100) {
                    return "0" + num.toString();
                }
                return num.toString();
            },
            runStart() {
                axios.get('https://shopping.geocities.jp/kenkenanto/json/smp/sec_kill.json')
                // axios.get("/static/json/smp/sec_kill.json")
                    .then(this.getJsonListSucc);
                },getJsonListSucc(res) {
                    var result = res.data;
                    if (result.ret && result.data) {
                    this.sec_kill = result.data.sec_kill;
                    this.banner_1 = result.data.banner_1;
                    this.banner_2 = result.data.banner_2;
                    this.secRun()
                }
            },
            secRun(){
                var now_sec = moment().format('ss')
                var today = moment().format('YYYY-MM-DD HH:mm:ss')
                this.sec_kill.forEach(e => {
                    // console.log(e)
                    var start = e.date +' '+ e.start_time
                    var end = e.date +" "+ e.end_time
                    var a1 = moment(start);
                    var a2 = moment(today);
                    var a3 = moment(end);
                    
                    var is_start = a1.diff(today, 'second')
                    var is_end = a2.diff(end, 'second')
                    if(is_end < 0){
                        if(is_start < 0){ // start
                            e.status = 'start'
                            e.dd = a3.diff(today, 'day')
                            e.hh = a3.diff(today, 'hour')%24
                            e.mm = a3.diff(today, 'minutes')%60
                            e.dd = this.zeroTwo(e.dd);
                            e.hh = this.zeroTwo(e.hh);
                            e.mm = this.zeroTwo(e.mm);
                            e.ss = 59 - now_sec
                            e.ss = this.zeroTwo(e.ss);
    
                            setInterval(() => {
                                e.xxx = 1000-Math.floor(Date.now())%1000;
                                e.xxx = this.zeroThree(e.xxx);
                            },20)
                        }else{  // ready
                            e.status = 'ready'
                            e.dd = a1.diff(today, 'day')
                            e.hh = a1.diff(today, 'hour')%24
                            e.mm = a1.diff(today, 'minutes')%60
                            e.dd = this.zeroTwo(e.dd);
                            e.hh = this.zeroTwo(e.hh);
                            e.mm = this.zeroTwo(e.mm);
                            e.ss = 59 - now_sec
                            e.ss = this.zeroTwo(e.ss);
    
                            setInterval(() => {
                                e.xxx = Math.floor(Date.now())%1000;
                                e.xxx = this.zeroThree(e.xxx);
                            },20)
                        }
                    }else{
                        e.status = 'close'
                        e.dd = '00'
                        e.hh = '00'
                        e.mm = '00'
                        e.ss = '00'
                        e.xxx = '000'
                    }
                });
            }
        },
        mounted() {
            var _this = this
            this.runStart()
            setInterval(() => {
                _this.secRun()
            }, 1000);
        }
    }
</script>

<style lang="stylus" scoped>
a
    color #000
    text-decoration none
a:hover
    color red
.end_txt
    margin-top 6rem
    font-weight bold
    font-size 1.2rem
.close_item
    position absolute
    width 100%
    height 100%
    text-align center
    color #FFF
    background rgba(0,0,0,.3)
.ready_item
    position absolute
    width 100%
    height 100%
    text-align center
    color #FFF
    background rgba(0,175,35,.3)
.p_panel
    padding .1rem .16rem
    background #ff4445
    color #ffffff
    font-weight bold
    font-size 1rem
    border-radius .35rem
.p_time
    color #FFF
    padding .3rem 0
    line-height 1rem
    background #ff4445
    font-weight bold
.f_left
    float left
.f_right
    float right
.p_txt
    font-size 12px
    height 50px
    text-align left
    line-height 16px;
    overflow hidden
.o_price
    color #666
    text-decoration line-through
    font-size 1 rem
    margin-right .4rem
.price
    color red
    margin-left .6rem
    font-size 1.2rem
.line
    height 2px
    background #2d8dc3
.center
    text-align center
.clear
    clear both
.sec_item
    margin-top 1rem
    position relative
    float left
    padding 0rem 0 .6rem 0
    width 22.5%
    border-radius .3rem
    margin-left 2%
    background #FFF
.sec_item:hover
    background #FFF
.banner
    width 100%
img
    width 100%
.box
    margin:0 1%;
.mt10
    margin-top 8px;
</style>
