<template>
	<div id="main">
		<div class="room-info-1">
			<div class="a_card_left">
		        <ul>
		            <li>
		            	<div  style="position:relative;">
		                	<img class="room-img" width="100%" :src="roomDetail.roomThumb"/>
		            		<div class="img-text-hover" style="position:absolute; z-index:1; left:82px; top:49px;background-color:transparent;opacity:0.35;display: none;">
		            			<a target="_blank" :href="'https://www.douyu.com/'+roomDetail.roomId">
		            				<el-button plain>进入直播间</el-button>
		            			</a>
		            		</div>
		            	</div>
		            </li>
		           <!-- <li>
		            	<a href="javascript:;" v-if="connected">
							<span v-if="roomConnecting===true"><i class="iconfont icon-jiazai"></i></span>
			        		<span v-else @click="disConnect()">断开连接</span>
						</a>
					    <a href="javascript:;"  v-else @click="connect()">
					    	<span v-if="roomConnecting===true"><i class="iconfont icon-jiazai"></i></span>
			        		<span v-else @click="disConnect()">连接</span>
					    </a>
		            </li>-->
		        </ul>
		    </div>
	    <div class="a_card_right">
	    	<div class="a_card_r1">
	            <h2>主播信息</h2>
	        </div>
	        <div class="a_card_r2">
	            <ul>
	                <li>
	                    <dl>
	                        <dd>
                                <label>房间号：</label>
	                        </dd>
	                        <dd>
	                            <p>{{roomDetail.roomId}}</p>
	                        </dd>
	                    </dl>
	                </li>
	                <li>
	                    <dl>
	                        <dd>
	                            <label>所属分类：</label>
	                        </dd>
	                        <dd>
	                            <p>{{roomDetail.cateName}}</p>
	                        </dd>
	                    </dl>
	                </li>
	                <li>
	                    <dl>
	                        <dd>
	                            <label>主播名：</label>
	                        </dd>
	                        <dd>
	                            <p>
	                                {{roomDetail.ownerName}}
	                            </p>
	                        </dd>
	                    </dl>
	                </li>
	                <li>
	                    <dl>
	                        <dd>
	                             <label>粉丝数：</label>
	                        </dd>
	                        <dd>
	                            <p>
	                                {{roomDetail.fansNum}}
	                            </p>
	                        </dd>
	                    </dl>
	                </li>
	            </ul>
	        </div>
	        <div class="a_card_r1 a_card_r3">
		            <div class="a_card_r1">
		                <h2>主播指数</h2>
		            </div>
		            <ul class="rank_j">
		                <li>
		                    <span><img v-show="isShow" src="../../static/images/loading.gif"/>{{chatTotalCount|numTransform}}条</span>
		                    <span class="dot"></span>
		                    <span class="txt">今日弹幕条数</span>
		                </li>
		                <li>
		                    <span><img v-show="isShow" src="../../static/images/loading.gif"/>{{aggregate.giftUserCounts}}人</span>
		                    <span class="dot"></span>
		                    <span class="txt">今日送礼人数</span>
		                </li>
		                <li>
		                    <span><img v-show="isShow" src="../../static/images/loading.gif"/>{{aggregate.userCounts}}人</span>
		                    <span class="dot"></span>
		                    <span class="txt">今日弹幕人数</span>
		                </li>
		                <li>
		                    <span><img v-show="isShow" src="../../static/images/loading.gif"/>{{aggregate.giftSum}}元</span>
		                    <span class="dot"></span>
		                    <span class="txt">今日礼物收入</span>
		                </li>
		            </ul>
	        </div>
	    </div>
		</div>
		<div class="second-container clearfix">
			<div class="gift-left">
				<div style="color: rgb(90, 45, 255);margin-bottom: 10px;width: 100%;">今日土豪榜</div>
				<div v-if="giftTop.length>0" class="gift-list">
					<div v-for="item in giftTop" v-bind:key="item.uid" style="float: left;width: 100%;height: 50px;text-overflow: ellipsis;overflow: hidden;word-wrap: break-word;">
						<img :src="'https://apic.douyucdn.cn/upload/'+item.ic+'_middle.jpg'" width="48px" height="48px" style="float: left;"/>
						<div style="font-size: 12px;padding:2px 11px 11px 59px;text-align: left;">
							用户
							<span style="color: rgb(90, 45, 255);padding-left: 0;font-size: 14px;">{{item.nn}}</span>
							共送出
							<span style="font-size: 16px;padding-left: 0;color: #f03a4a;">{{item.value}}</span>
							元礼物
						</div>
					</div>
				</div>
				<div v-else>
					无数据
				</div>
				<div style="color: rgb(90, 45, 255);margin-bottom: 10px;width: 100%;">今日最新礼物</div>
				<div v-if="gifts.length>0" class="gift-list">
					<div v-for="gift in gifts" v-bind:key="gift.id" style="float: left;width:100%;height: 50px;text-overflow: ellipsis;overflow: hidden;word-wrap: break-word;">
						<div style="width: 50px;float: left;"><img :alt="gift.pc+' rmb'" v-bind:src="roomGifts[gift.gfid]!=null?roomGifts[gift.gfid].himg:tableGift[gift.pc]!=undefined&&tableGift[gift.pc]!=null?tableGift[gift.pc]:''" height="48px;" width="48px"/></div>
						<div style="width: auto;padding: 2px 0 0 59px;text-align:left;">
							<span>x{{gift.gfcnt}}</span>
							用户
							<span style="color: rgb(90, 45, 255);font-size: 14px;">{{gift.nn}}</span>&nbsp;&nbsp;送出<span style="font-size: 16px;">{{gift.pc}}</span>鱼翅
						</div>
					</div>
				</div>
				<div v-else>
					无数据
				</div>
			</div>
			<div class="chat-center">
				<div class="nav_tab clearfix nav_tab1">
	                    <ul class="first-tab">
	                        <li v-bind:class="{active:active==0}" @click="getViewdata(0)">礼物收入</li>
	                        <li v-bind:class="{active:active==1}" @click="getViewdata(1)">送礼人数</li>
	                        <li v-bind:class="{active:active==2}" @click="getViewdata(2)">弹幕条数</li>
	                        <li v-bind:class="{active:active==3}" @click="getViewdata(3)">弹幕人数</li>
	                        <li v-bind:class="{active:active==4}" @click="getViewdata(4)">粉丝人数</li>
	                    </ul>
	                    <ul class="second-day">
	                    	<li v-bind:class="{active:dateActive==30}" @click="changeViewDate(30)">最近三十天</li>
	                    	<li v-bind:class="{active:dateActive==7}" @click="changeViewDate(7)">最近七天</li>
		                </ul>
                </div>
				<div id='view'  v-loading="viewLoading">
					<div class="room-view"  v-show="active==0" ref="giftChart"></div>
					<div class="room-view" v-show="active==1" ref="giftPersonChart"></div>
					<div class="room-view"  v-show="active==2" ref="chatNumChart"></div>
					<div class="room-view" v-show="active==3" ref="chatPersonChart"></div>
					<div class="room-view" v-show="active==4" ref="fansNumChart"></div>
				</div>
				<div style="border-bottom: 2px rgb(90, 45, 255) solid;width: 100%;color: rgb(90, 45, 255);">今日最新弹幕<span style="float: right;padding-right: 10px;" @click="getMoreChat()"><a href="javascript:;" style="color: darkorchid;text-decoration: none;">查看更多</a></span></div>
				<div class="chat-list">
					<div class="chat-item" v-for="chat in chats" v-bind:key="chat.id">
							<div style="float: left;margin-left: 6px;margin-top: 8px;height: 36px;overflow: hidden; text-overflow: ellipsis; white-space: nowrap;">
								<span>{{chat.timestamp}}</span>
								<a v-bind:class="'user-level level-bgpng level-size1 level-'+chat.level" v-bind:title="'用户等级：'+chat.level"></a>
								<span style="color: #2b94ff;padding-left: 5px;margin-left: 40px;">{{chat.nn}}：</span>
								<span v-if="chat.ifs==1" v-bind:style="'color:'+tableColor[chat.col]+' ;'">{{chat.txt}}</span>
								<span v-else style="color: #333;">{{chat.txt}}</span>
							</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	import tableColor from '../config/tableColor.json'
	import tableGift from '../config/tableGift.json'
	import Vue from 'vue'
	//引入模块
	var echarts = require('echarts');
	import echartsOptions from '../utils/echartsOptions.js'
	export default {
		name:'RoomDetail',
		data(){
			return{
				tableGift:tableGift,
				tableColor:tableColor,
				roomId:null,
				chatTotalCount:null,
				roomDetail:{},
				gifts:[],
				chats:[],
				roomGifts:{},
				giftTop:[],
				aggregate:{},
				active:0,
				dateActive:7,
				viewLoading:true,
				isShow:true,
				viewData:{
					0:{'method':this.getViewGiftMoneyData,ref:'giftChart','chart7':null,'chart30':null},
					1:{'method':this.getViewGiftPersonNumData,ref:'giftPersonChart','chart7':null,'chart30':null},
					2:{'method':this.getViewChatSumData,ref:'chatNumChart','chart7':null,'chart30':null},
					3:{'method':this.getViewChatPersonNumData,ref:'chatPersonChart','chart7':null,'chart30':null},
					4:{'method':this.getViewFansPersonNumData,ref:'fansNumChart','chart7':null,'chart30':null},
				}
			}
		},
		created(){
			var roomId=this.$route.params.roomId;
			this.roomId=roomId;
			this.getRoomDetail(roomId);
		},
		mounted() {
			var $this=this;
			this.getViewdata(0);
			//图表自适应
			window.onresize = function(){
				var chart7=$this.viewData[$this.active]['chart7'];
				if(chart7!=null){
				   chart7.resize();
				}
				var chart30=$this.viewData[$this.active]['chart30'];
				if(chart30!=null){
				   chart30.resize();
				}
			};
			$(".room-img").mouseover(function(event){
				$(".img-text-hover").show();
			}).mouseout(function(event){
				$(".img-text-hover").hide();
			});
			$(".img-text-hover").mouseover(function(event){
				$(".img-text-hover").show();
			});
		},
		beforeDestroy() {
		    var viewData=this.viewData;
		    for (let key in viewData) {
		    	var chart30=viewData[key]['chart30'];
		    	if(!chart30){
		    		return;
		    	}
		    	chart30.dispose();
		   		chart30 = null;
		   		var chart7=viewData[key]['chart7'];
		    	if(!chart7){
		    		return;
		    	}
		    	chart7.dispose();
		   		chart7 = null;
		    }
		   
		},
		methods:{
			initParam(){
				if(this.dateActive==7){
					return this.getSevenDayIntervalParams();
				}else if(this.dateActive==30){
					return this.getThirtyDayIntervalParams();
				}
			},
			//7天内
			getSevenDayIntervalParams(){
				var end=new Date();
				var start = new Date();
				start.setDate(start.getDate()-6);
				return {
					"rid":this.roomId,
					"end":Vue.filter('util').formatDate(end,"yyyy-MM-dd"),
					"start":Vue.filter('util').formatDate(start,"yyyy-MM-dd")
				};
			},
			//30天内
			getThirtyDayIntervalParams(){
				var end=new Date();
				var start = new Date();
				start.setDate(start.getDate()-29);
				return {
					"rid":this.roomId,
					"end":Vue.filter('util').formatDate(end,"yyyy-MM-dd"),
					"start":Vue.filter('util').formatDate(start,"yyyy-MM-dd")
				};
			},
			//切换
			getViewdata(active){
				this.$set(this,'active',active);
				if(this.dateActive==7){
					if(!this.viewData[active]['chart7']){
						this.$set(this,'viewLoading',true);
					}else{
						var chart=this.viewData[active]['chart7'];
						var chart1=echarts.init(this.$refs[this.viewData[active]['ref']],null,{renderer: 'cavas'});
					}
					this.viewData[active]['method'].call()
				}else if(this.dateActive==30){
					if(!this.viewData[active]['chart30']){
						this.$set(this,'viewLoading',true);
					}
					this.viewData[active]['method'].call()
				}
			},
			changeViewDate(dateInterval){
				this.$set(this,'dateActive',dateInterval);
				this.getViewdata(this.active);
			},
			//礼物收入
			getViewGiftMoneyData(){
				var $this=this;
				var room=this.roomId;
				var p=this.initParam();
				var params=new URLSearchParams();
				params.append("rid",p.rid);
				params.append("start",p.start);
				params.append("end",p.end);
				params={
					rid:p.rid,
					start:p.start,
					end:p.end
				}
				this.$request.getGiftMoney(p.rid,params)
		  		.then((response)=>{
		  			var res=response;
					let x=new Array();
					let y=new Array();
					for (let key in res.body) {
						x.push(key);
					}
					x.sort(function(o1,o2){
						var d1=new Date(o1);
						var d2=new Date(o2);
						return d1>d2?1:(d1==d2?0:-1);
					})
					x.forEach((item,index)=>{
						y.push(res.body[item].toFixed(2));
					})
					var chart=echarts.init($this.$refs.giftChart,null,{renderer: 'cavas'});
					var option=echartsOptions.giftMoney;
					if(!x.length||!y.length){
						option.title.text=option.title.text+"\n\n无数据";
					}
					option.xAxis.data=x;
					option.series[0].data=y;
					chart.setOption(option);
					$this.$set($this,'viewLoading',false);
					if($this.dateActive==7){
						$this.viewData[0]['chart7']=chart;
					}else if($this.dateActive==30){
						$this.viewData[0]['chart30']=chart;
					}
		  		});
			},
			//礼物人数
			getViewGiftPersonNumData(){
				var $this=this;
				var room=this.roomId;
				var p=this.initParam();
				var params=new URLSearchParams();
				params.append("rid",p.rid);
				params.append("start",p.start);
				params.append("end",p.end);
				params={
					rid:p.rid,
					start:p.start,
					end:p.end
				}
				this.$request.getGiftNum(p.rid,params)
				.then((response)=>{
					var res=response;
					let x=new Array();
					let y=new Array();
					for (let key in res.body) {
						x.push(key);
					}
					x.sort(function(o1,o2){
						var d1=new Date(o1);
						var d2=new Date(o2);
						return d1>d2?1:(d1==d2?0:-1);
					})
					x.forEach((item,index)=>{
						y.push(res.body[item]);
					})
					var chart=echarts.init($this.$refs.giftPersonChart,null,{renderer: 'cavas'});
					var option=echartsOptions.giftPerson;
					if(!x.length||!y.length){
						option.title.text=option.title.text+"\n\n无数据";
					}
					option.xAxis.data=x;
					option.series[0].data=y;
					chart.setOption(option);
					$this.$set($this,'viewLoading',false);
					if($this.dateActive==7){
						$this.viewData[1]['chart7']=chart;
					}else if($this.dateActive==30){
						$this.viewData[1]['chart30']=chart;
					}
				});
			},
			//弹幕条数
			getViewChatSumData(){
				var $this=this;
				var room=this.roomId;
				var p=this.initParam();
				var params=new URLSearchParams();
				params.append("rid",p.rid);
				params.append("start",p.start);
				params.append("end",p.end);
				this.$request.getChatNum(p.rid,params)
				.then((response)=>{
					var res=response;
					let x=new Array();
					let y=new Array();
					for (let key in res.body) {
						x.push(key);
					}
					x.sort(function(o1,o2){
						var d1=new Date(o1);
						var d2=new Date(o2);
						return d1>d2?1:(d1==d2?0:-1);
					})
					x.forEach((item,index)=>{
						y.push(res.body[item]);
					})
					var chart=echarts.init($this.$refs.chatNumChart,null,{renderer: 'cavas'});
					var option=echartsOptions.chatNum;
					if(!x.length||!y.length){
						option.title.text=option.title.text+"\n\n无数据";
					}
					option.xAxis.data=x;
					option.series[0].data=y;
					chart.setOption(option);
					$this.$set($this,'viewLoading',false);
					if($this.dateActive==7){
						$this.viewData[2]['chart7']=chart;
					}else if($this.dateActive==30){
						$this.viewData[2]['chart30']=chart;
					}
				});
			},
			//弹幕人数
			getViewChatPersonNumData(){
				var $this=this;
				var room=this.roomId;
				var p=this.initParam();
				var params=new URLSearchParams();
				params.append("rid",p.rid);
				params.append("start",p.start);
				params.append("end",p.end);
				this.$request.getChatSum(p.rid,params)
				.then((response)=>{
					var res=response;
					let x=new Array();
					let y=new Array();
					for (let key in res.body) {
						x.push(key);
					}
					x.sort(function(o1,o2){
						var d1=new Date(o1);
						var d2=new Date(o2);
						return d1>d2?1:(d1==d2?0:-1);
					})
					x.forEach((item,index)=>{
						y.push(res.body[item]);
					})
					var chart=echarts.init($this.$refs.chatPersonChart,null,{renderer: 'cavas'});
					var option=echartsOptions.chatPerson;
					if(!x.length||!y.length){
						option.title.text=option.title.text+"\n\n无数据";
					}
					option.xAxis.data=x;
					option.series[0].data=y;
					chart.setOption(option);
					$this.$set($this,'viewLoading',false);
					if($this.dateActive==7){
						$this.viewData[3]['chart7']=chart;
					}else if($this.dateActive==30){
						$this.viewData[3]['chart30']=chart;
					}
				});
			},
			//粉丝人数
			getViewFansPersonNumData(){
				var $this=this;
				var room=this.roomId;
				var p=this.initParam();
				var params=new URLSearchParams();
				params.append("rid",p.rid);
				params.append("start",p.start);
				params.append("end",p.end);
				this.$request.getFansNum(p.rid,params)
				.then((response)=>{
					var res=response;
					let x=new Array();
					let y=new Array();
					res.body.forEach((item,index)=>{
						x.push(item[0]);
						y.push(item[1]);
					});
					var chart=echarts.init($this.$refs.fansNumChart,null,{renderer: 'cavas'});
					var option=echartsOptions.fansPerson;
					if(!x.length||!y.length){
						option.title.text=option.title.text+"\n\n无数据";
					}
					option.xAxis.data=x;
					option.series[0].data=y;
					chart.setOption(option);
					$this.$set($this,'viewLoading',false);
					if($this.dateActive==7){
						$this.viewData[4]['chart7']=chart;
					}else if($this.dateActive==30){
						$this.viewData[4]['chart30']=chart;
					}
				});
			},
			getRoomDetail(roomId){
				var $this=this;
				this.$request.getRoomInfo(roomId)
					.then((response)=>{
						$this.isShow=false;
						var res=response;
						var body=res.body;
						$this.roomDetail=body.roomDetail;
						$this.chatTotalCount = body.chatTotalCount;
						$this.chats=body.chats;
						$this.gifts=body.gifts;
						$this.aggregate=body.aggregate;
						var json=new Object();
						$.each(body.roomDetail.roomGifts, function(i,n) {
							json[n.id]=n;
						});
						$this.roomGifts=json;
						$this.connected=body.connected;
						body.giftTop.sort(function(o1,o2){
							return o1.value>o2.value?-1:o1.value==o2.value?0:1;
						})
						$.each(body.giftTop, function(i,n) {
							n.value=n.value.toFixed(0);
						});
						$this.giftTop=body.giftTop;
					})
			},
			getMoreChat(){
				var roomId=this.roomId;
				var path='/danmu';
				this.$router.push({path:path,query:{roomId:roomId}});
			},
		}
	}
</script>
<style scoped="scoped">
	@import '../../static/css/level.css';
</style>
<style scoped="scoped">
	#main{
		margin:auto;
	}
	.room-info-1{
		height: 202px;
	    border: 1px solid #E6E6E6;
	    margin-top: 20px;
	    width: 1200px;
	    margin: 0 auto;
	    position: relative;
	}
	.a_card_left{
	    width: 280px;
	    height: 100%;
	    color: white;
	    padding: 10px;
	    float: left;
	}
	.a_card_right{
		float: right;
    	width: 900px;
	}
	.a_card_right .a_card_r1 {
	    height: 50px;
	}
	.a_card_right .a_card_r1 h2 {
	    float: left;
	    line-height: 50px;
	    font-size: 16px;
	    color: #333333;
	}
	.a_card_r2 li {
	    display: inline-block;
	    width: 222px;
	    text-align: center;
	    margin-left: -4px;
	    margin-top: 20px;
	}
	.a_card_r2 li dd {
	    display: inline-block;
	    vertical-align: middle;
	    font-size: 14px;
    	color: #666666;
	}
	.a_card_r3 ul {
	    margin-top: 3px;
	   float: left;
	}
	.a_card_r3 ul li {
	    display: inline-block;
	    width: 205px;
	    text-align: center;
	    margin-left: 7px;
	}
	.a_card_r3 ul li span {
	    font-size: 15px;
	    color: #f03a4a;
	    line-height: 16px;
	    position: relative;
	    text-align: center;
	    display: block;
	}
	.a_card_r3 ul li span.dot {
	    background: url(../../static/images/card_dian.png)0 0 no-repeat;
	    width: 8px;
	    height: 8px;
	    display: block;
	    margin: 0 auto;
	}
	.a_card_r3 ul li .txt {
	    font-size: 12px;
	    color: #666666;
	    line-height: 24px;
	}
	#view{
		border-bottom: 1px solid #E6E6E6;
		border-top: 2px rgb(90, 45, 255) solid;
	}
	.room-view{
		height: 26.87rem;
		width: 828px;
	}
	.second-container{
	    width: 1200px;
	    margin: 0 auto;
	    position: relative;
	}
	.gift-left{
		height: auto;
		padding: 10px;
		float: left;
		background: white;
		width: 300px;
		border: 1px solid #E6E6E6;
	}
	.gift-list span{
		padding-left: 0.18rem;
	}
	.chat-center{
		float: right;
		width: 868px;
		height: auto;
	}
	.chat-list .chat-item{
		width: 100%;
		height: 2.12rem;
		float: left;
	}
	a{
	  color: #f56c6c;
	  font-size: 0.87rem;
	  letter-spacing: 0;
	  text-decoration: none;
	}
	.clearfix {
	    zoom: 1;
	    clear: both;
	}
	.clearfix:after {
	    content: ".";
	    display: block;
	    height: 0;
	    clear: both;
	    visibility: hidden;
	}
	.nav_tab ul.first-tab li {
	    border-right: 1px solid white;
        background: #f3f3f3;
	    padding: 6px 10px;
	    float: left;
	    font-size: 12px;
	    color: #666666;
	    cursor: pointer;
	}
	.nav_tab ul.first-tab li.active{
		background-color: rgb(90, 45, 255);
		color: #F3F3F3;
	}
	.nav_tab ul.second-day{
		display: inline;
	}
	.nav_tab ul.second-day li:last-child{
		 border-right: 1px solid #E6E6E6;
	}
	.nav_tab ul.second-day li.active{
    	color: #f56c6c;
	}
	.nav_tab ul.second-day li{
	    font-size: 12px;
	    color: #333333;
	    float: right;
	    cursor: pointer;
	    width: 67px;
	    padding: 4px;
	    margin-top: 2px;
		text-align: center;
	}
	.el-button.is-plain:focus, .el-button.is-plain:hover{
		border-color: ghostwhite;
		color: #0000FF;
	}
</style>