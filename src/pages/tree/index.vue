<template>
    <div class="tree">
        <div class="choose-date" @click="showModel">选择日期</div>
        <div  :class="{'zan-dialog-show' :popupVisible}">
            <div class="popup" v-if="popupVisible" @click="closeModel">
            </div>
            <div class="popup-wraper service-popup-wraper">
                <div class="po-rela">
                    <div class="popup-fixed-title">
                        <div class="blue" @click="closeModel">关闭</div>
                        <div>日期选择</div>
                        <div class="blue" @click="reset">清空</div>
                    </div>
                    <div class="popup-fixed-dateNum">
                        <div class="ruzhu-date">
                            <span>入住时间</span>
                            <p class="checkInTime">{{checkInTime}}</p>
                        </div>
                        <div class="date-num">
                            共<span>{{num}}</span>晚
                        </div>
                        <div class="tuifang-date">
                            <span>退房时间</span>
                            <p class="checkOutTime">{{checkOutTime}}</p>
                        </div>
                    </div>
                    <div class="weekday">
                        <div class="weekday-item" v-for="item in weekday" :key="item">{{item}}</div>
                    </div>
                    <div class="change-month">
                        <span @click="prevMonth">prev</span>
                        <span>{{dateArray[fistIndex].month}}</span>
                        <span @click="nextMonth">next</span>
                    </div>
                    <date-component :daysArray="dateArray[fistIndex]" @chooseDate="getChooseDate" @nochoose="noChoose"></date-component>
                    <div class="ebooking" v-if="btnVisiby" @click="booking">{{yuding}}</div>
                    <div class="ebooking-error" v-if="!btnVisiby">{{errorText}}</div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import dateComponent from "@/components/date-component";
import util from "../../utils/util"
export default {
    components:{
        dateComponent
    },
    data(){
        return{
                popupVisible:false,
                checkInTime:"请选择",
                checkOutTime:"请选择",
                weekday:["日","一","二","三","四","五","六"],
                dateList:[],
                fistIndex:0,
                totalPrice:0,
                btnVisiby:true,
                num:0,
                yuding:"立即预定",
                errorText:"您所选的日期包含满房日期",
                dateArray:[
                    {
                        month:"2019年8月",
                        days:[
                            {date:"2019/08/01",price:"323"}, {date:"2019/08/02",price:"323"},{date:"2019/08/03",price:"323"}, {date:"2019/08/04",price:"323"}, {date:"2019/08/05",price:"323"},
                            {date:"2019/08/06",price:"323"}, {date:"2019/08/07",price:"323"}, {date:"2019/08/08",price:"323"}, {date:"2019/08/09",price:"323"}, {date:"2019/08/10",price:"323"},
                            {date:"2019/08/11",price:"323"}, {date:"2019/08/12",price:"323"}, {date:"2019/08/13",price:"323"}, {date:"2019/08/14",price:"323"},{date:"2019/08/15",price:"323"},
                            {date:"2019/08/16",price:"323"}, {date:"2019/08/17",price:"323"}, {date:"2019/08/18",price:"323"}, {date:"2019/08/19",price:"323"},{date:"2019/08/20",price:"323"},
                            {date:"2019/08/21",price:"323"},{date:"2019/08/22",price:"323"},{date:"2019/08/23",price:"323"},{date:"2019/08/24",price:"323"},{date:"2019/08/25",price:"323"},
                            {date:"2019/08/26",price:"323"},{date:"2019/08/27",price:"323"},{date:"2019/08/28",price:"323"},{date:"2019/08/29",price:"323"},{date:"2019/08/30",price:"323"},
                            {date:"2019/08/31",price:"323"}

                        ]
                    },
                    {
                        month:"2019年9月",
                        days:[
                            {date:"2019/09/01",price:"323"}, {date:"2019/09/02",price:"323"},{date:"2019/09/03",price:"323"}, {date:"2019/09/04",price:"323"}, {date:"2019/09/05",price:"323"},
                            {date:"2019/09/06",price:"323",full:true}, {date:"2019/09/07",price:"323"}, {date:"2019/09/08",price:"323"}, {date:"2019/09/09",price:"323"}, {date:"2019/09/10",price:"323"},
                            {date:"2019/09/11",price:"323"}, {date:"2019/09/12",price:"323"}, {date:"2019/09/13",price:"323"}, {date:"2019/09/14",price:"323"},{date:"2019/09/15",price:"323"},
                            {date:"2019/09/16",price:"323"}, {date:"2019/09/17",price:"323",full:true}, {date:"2019/09/18",price:"323"}, {date:"2019/09/19",price:"323"},{date:"2019/09/20",price:"323"},
                            {date:"2019/09/21",price:"323"},{date:"2019/09/22",price:"323"},{date:"2019/09/23",price:"323"},{date:"2019/09/24",price:"323"},{date:"2019/09/25",price:"323"},
                            {date:"2019/09/26",price:"323"},{date:"2019/09/27",price:"323",full:true},{date:"2019/09/28",price:"323",full:true},{date:"2019/09/29",price:"323"},{date:"2019/09/30",price:"323"}
                        ]
                    },
                    {
                        month:"2019年10月",
                        days:[
                            {date:"2019/10/01",price:"323"}, {date:"2019/10/02",price:"323"},{date:"2019/10/03",price:"323"}, {date:"2019/10/04",price:"323"}, {date:"2019/10/05",price:"323"},
                            {date:"2019/10/06",price:"323",full:true}, {date:"2019/10/07",price:"323"}, {date:"2019/10/08",price:"323"}, {date:"2019/10/09",price:"323"}, {date:"2019/10/10",price:"323"},
                            {date:"2019/10/11",price:"323"}, {date:"2019/10/12",price:"323",full:true}, {date:"2019/10/13",price:"323",full:true}, {date:"2019/10/14",price:"323"},{date:"2019/10/15",price:"323"},
                            {date:"2019/10/16",price:"323"}, {date:"2019/10/17",price:"323"}, {date:"2019/10/18",price:"323"}, {date:"2019/10/19",price:"323"},{date:"2019/10/20",price:"323"},
                            {date:"2019/10/21",price:"323"},{date:"2019/10/22",price:"323"},{date:"2019/10/23",price:"323",full:true},{date:"2019/10/24",price:"323"},{date:"2019/10/25",price:"323"},
                            {date:"2019/10/26",price:"323"},{date:"2019/10/27",price:"323"},{date:"2019/10/28",price:"323"},{date:"2019/10/29",price:"323"},{date:"2019/10/30",price:"323"},
                            {date:"2019/10/31",price:"323"}
                        ]
                    },
                    {
                        month:"2019年11月",
                        days:[
                            {date:"2019/11/01",price:"323"}, {date:"2019/11/02",price:"323"},{date:"2019/11/03",price:"323"}, {date:"2019/11/04",price:"323"}, {date:"2019/11/05",price:"323"},
                            {date:"2019/11/06",price:"323"}, {date:"2019/11/07",price:"323"}, {date:"2019/11/08",price:"323"}, {date:"2019/11/09",price:"323"}, {date:"2019/11/10",price:"323"},
                            {date:"2019/11/11",price:"323"}, {date:"2019/11/12",price:"323"}, {date:"2019/11/13",price:"323"}, {date:"2019/11/14",price:"323"},{date:"2019/11/15",price:"323"},
                            {date:"2019/11/16",price:"323"}, {date:"2019/11/17",price:"323"}, {date:"2019/11/18",price:"323"}, {date:"2019/11/19",price:"323"},{date:"2019/11/20",price:"323"},
                            {date:"2019/11/21",price:"323"},{date:"2019/11/22",price:"323"},{date:"2019/11/23",price:"323"},{date:"2019/11/24",price:"323"},{date:"2019/11/25",price:"323"},
                            {date:"2019/11/26",price:"323"},{date:"2019/11/27",price:"323"},{date:"2019/11/28",price:"323"},{date:"2019/11/29",price:"323"},{date:"2019/11/30",price:"323"}
                        ]
                    }
                ],
                allDays:[]
        }
    },
    mounted(){
        this.getAllDates();
    },
    created(){
        console.log("created");
    },
    methods:{
        reset(){
            wx.removeStorageSync("hotelChooseDate");
            wx.removeStorageSync("hotelFirstDate");
            this.checkInTime="";
            this.checkOutTime="";
            this.btnVisiby=true;
            this.yuding="立即预定";
            this.totalPrice=0;
            this.num=0;
            this.dateList=[];
            for(let i=0;i<this.dateArray.length;i++){
                let _item=this.dateArray[i].days;
                for(let j=0;j<_item.length;j++){
                    _item[j].isEndDay=false;
                    _item[j].isFirstDay=false;
                    _item[j].ischeckd=false;
                }
            }
        },
        getChooseDate(data){
            console.log(data);
            let arr=data;
            this.totalPrice=0;
            if(Array.isArray(arr)){
                this.num=data.length-1;
                this.checkInTime=data[0];
                this.checkOutTime=data[data.length-1];
                this.dateList=JSON.parse(JSON.stringify(arr));
                let getlist=JSON.parse(JSON.stringify(arr));
                getlist.pop();
                for(let i=0;i<this.allDays.length;i++){
                    if(getlist.includes(this.allDays[i].date) && this.allDays[i].full!==true){
                        this.totalPrice+=this.allDays[i].price*1;
                    }else if(getlist.includes(this.allDays[i].date) && this.allDays[i].full===true){
                        this.btnVisiby=false;
                        this.totalPrice=0;
                        return
                    }
                }
                console.log(this.totalPrice);
                if(this.totalPrice===0){
                    this.btnVisiby=false;
                    this.yuding="立即预定";
                }else{
                    this.btnVisiby=true;
                    this.yuding="￥"+this.totalPrice+" 立即预定"
                }
            }
        },
        booking(){
            if(this.dateList.length!==0){
                console.log(this.dateList);
                console.log(this.totalPrice);
            }else{
                util.showErrorToastMessage("请选择入住日期");
                return
            }
        },
        showModel(){
            this.popupVisible=!this.popupVisible;
        },
        closeModel(){
            this.popupVisible=false;
        },
        noChoose(){
            this.yuding="立即预定";
        },
        getAllDates(){
            let week=["日","一","二","三","四","五","六"];
            let today=new Date().getFullYear()+"/"+(new Date().getMonth()+1)+ "/"+  new Date().getDate();
             let _list=this.dateArray;
            for(let i=0;i<_list.length;i++){
                let _item=_list[i].days;
                let _monthF=_item[0];
                let firstDay=new Date(Date.parse(_monthF.date));
                let firstWeek=firstDay.getDay();
                _list[i].days[0].marginleft=firstWeek*100+"rpx";
                for(let j=0;j<_item.length;j++){
                    if(new Date(_item[j].date).getTime()>= new Date(today).getTime()){
                        _item[j].afterToday=true
                    }else{
                         _item[j].afterToday=false
                    }
                }
            }
            this.dateArray=_list;
            for(let i=0;i<this.dateArray.length;i++){
                this.allDays.push(...this.dateArray[i].days);
            }
            //console.log(this.allDays);
        },
        nextMonth(){
            if(this.fistIndex===this.dateArray.length-1){
                this.fistIndex=this.dateArray.length-1
            }else{
                this.fistIndex= this.fistIndex+1;
            }
        },
        prevMonth(){
            if(this.fistIndex>0){
                this.fistIndex=this.fistIndex-1*1
            }else{
                this.fistIndex=0
            }
        },
    }
}
</script>
<style lang="stylus" scoped>
.tree{
    width 100%;
    min-height:100vh;
    background-color #f2f2f2;
    .choose-date{
        width 100%
        height  80rpx
        line-height 80rpx
        text-align center
        color #333
        font-size 28rpx
        border-top 1rpx solid #dddddd
        border-bottom 1rpx solid #ddd
    }
}
.popup{
    width: 100%;
    height:100%;
    position: absolute;
    z-index: 99;
    left: 0;
    bottom: 0;
    background: rgba(0,0,0,0.2);
}
.popup-wraper{
    position: fixed;
    left: 0;
    bottom: 0;
    min-height: 800rpx;
    width: 750rpx;
    padding 25rpx 0 0 0;
    background: #fff;
    overflow-y: auto;
    z-index: 999;
    transform: translateY(150%);  
    transition: all 0.4s ease; 
    color #666
    .po-rela{
        position relative
    }
}
.zan-dialog-show .popup-wraper{
     transform: translateY(0);
  }
.popup-fixed-title{
    display  flex
    flex-flow row nowrap;
    justify-content space-between
    font-size 28rpx
    padding 0 25rpx
    .blue{
        color #2f86f7;
    }
}
.popup-fixed-dateNum{
    display flex
    flex-flow row nowrap
    justify-content space-around
    margin 28rpx 0;
    font-size 28rpx
    align-items center
    .ruzhu-date{
        display flex
        flex-flow column nowrap
        justify-content center
        align-items center
        .checkInTime{
            font-size 26rpx
            color #333
        }
    }
    .tuifang-date{
        display flex
        flex-flow column nowrap
        justify-content center
        align-items center
        .checkOutTime{
            color #333
            font-size 26rpx
        }
    }
    .date-num{
        border 1rpx solid #ddd
        height 48rpx
        display flex
        flex-flow row nowrap
        align-items center
        padding 0 32rpx
        border-radius 24rpx
    }
}
.weekday{
    display flex
    flex-flow row nowrap
    justify-content space-between
    position relative
    padding-bottom 16rpx
    width 700rpx
    margin-left 25rpx
    font-size 28rpx
    &:after{
        position absolute
        content ""
        bottom 0
        z-index 999
        left -25rpx
        right -25rpx
        height 2rpx
        -webkit-transform: scaleY(.5);
        transform:scaleY(.5);
        background-color #ddd
    }
    .weekday-item:first-child{
        color cadetblue
    }
    .weekday-item:last-child{
        color cadetblue
    }
}
.change-month{
    display flex
    flex-flow row nowrap
    justify-content space-between
    margin 20rpx
    font-size 28rpx
}
.ebooking{
    width 100%;
    height  80rpx;
    position absolute
    line-height 80rpx
    text-align center
    font-size 34rpx
    background-color: #2f86f7;
    position: absolute;
    z-index 999
    bottom: 2rpx;
    color: #fff;
}
.ebooking-error{
    width 100%;
    height  80rpx;
    position absolute
    line-height 80rpx
    text-align center
    font-size 34rpx
    background-color #d1d1d1
    position: absolute;
    z-index 999
    bottom: 2rpx;
    color: #fff;
}
</style>


