<template>
    <div class="orderstatusbox">
        <BackBar :dTitle='dTitle'></BackBar>

        <!--<mu-flex class="flex-wrapper shdz" align-items="center" @click='goAddDress'>
		    <mu-flex class="flex-demo" align-items="center" justify-content="center" fill>
				<img src="../../../static/img/car/ic_tianjia.png"/>
		    	<p class="title">
					添加收货地址
				</p>
		    </mu-flex>
		</mu-flex>-->

        <!--订单状态-->
        <div class="status">
            <!--等待付款-->
            <mu-paper v-if='status == 1' :z-depth="1" class="demo-list-wrap">
                <mu-list textline="three-line">
                    <div class="li-box">
                        <mu-list-item avatar :ripple="false" button>
                            <mu-list-item-action>
                                <mu-avatar style="width: 1.4rem;height: 1.4rem;">
                                    <img src="../../../static/img/order/img_dengdaifukuan.png">
                                </mu-avatar>
                            </mu-list-item-action>
                            <mu-list-item-content>
                                <mu-list-item-title>等待付款</mu-list-item-title>
                                <mu-list-item-sub-title>
                                    <span class="kc"></span>
                                    <p class="ostatus">
                                        <span class="statustime">剩29分钟后关闭</span>
                                    </p>
                                </mu-list-item-sub-title>
                            </mu-list-item-content>
                        </mu-list-item>
                        <!--<mu-divider></mu-divider>-->
                        <!-- <div style="position: absolute;right: 0;bottom: 0;">
					    	<div class="saoma">
					        	<span>x1</span>
					        </div>
					    </div>-->
                    </div>
                </mu-list>
            </mu-paper>

            <!--商品等待发货-->
            <mu-paper v-if='status == 2' :z-depth="1" class="demo-list-wrap">
                <mu-list textline="three-line">
                    <div class="li-box">
                        <mu-list-item avatar :ripple="false" button>
                            <mu-list-item-action>
                                <mu-avatar style="width: 1.4rem;height: 1.4rem;">
                                    <img src="../../../static/img/order/img_daifahuo.png">
                                </mu-avatar>
                            </mu-list-item-action>
                            <mu-list-item-content class='dfh'>
                                <mu-list-item-title>商品等待发货</mu-list-item-title>
                                <mu-list-item-sub-title>
                                    <span class="kc"></span>
                                    <!--<p class="ostatus">
					          	<span>剩29分钟后关闭</span>
					          </p>-->
                                </mu-list-item-sub-title>
                            </mu-list-item-content>
                        </mu-list-item>
                        <!--<mu-divider></mu-divider>-->
                        <!-- <div style="position: absolute;right: 0;bottom: 0;">
					    	<div class="saoma">
					        	<span>x1</span>
					        </div>
					    </div>-->
                    </div>
                </mu-list>
            </mu-paper>

            <!--商品已发货-->
            <mu-paper v-if='status == 3' :z-depth="1" class="demo-list-wrap">
                <mu-list textline="three-line">
                    <div class="li-box">
                        <mu-list-item avatar :ripple="false" button>
                            <mu-list-item-action>
                                <mu-avatar style="width: 1.4rem;height: 1.4rem;">
                                    <img src="../../../static/img/order/img_yifahuo.png">
                                </mu-avatar>
                            </mu-list-item-action>
                            <mu-list-item-content>
                                <mu-list-item-title>商品已发货</mu-list-item-title>
                                <mu-list-item-sub-title>
                                    <span class="kc"></span>
                                    <p class="ostatus" style="width: 80%;text-overflow: ellipsis;overflow: hidden;white-space: nowrap;">
                                        <span class="statustime">物流最新状态物流状态物流最新状态物流状态</span>
                                    </p>
                                </mu-list-item-sub-title>
                            </mu-list-item-content>
                        </mu-list-item>
                        <!--<mu-divider></mu-divider>-->
                        <div class="ckxq">
                            <div class="saoma" @click="goExpress">
                                <span style="color:#f95151">查看详情</span>
                            </div>
                        </div>
                    </div>
                </mu-list>
            </mu-paper>

            <!--交易完成-->
            <mu-paper v-if='status == 4' :z-depth="1" class="demo-list-wrap">
                <mu-list textline="three-line">
                    <div class="li-box">
                        <mu-list-item avatar :ripple="false" button>
                            <mu-list-item-action>
                                <mu-avatar style="width: 1.4rem;height: 1.4rem;">
                                    <img src="../../../static/img/order/img_jiaoyiwancheng.png">
                                </mu-avatar>
                            </mu-list-item-action>
                            <mu-list-item-content class='dfh'>
                                <mu-list-item-title>交易完成</mu-list-item-title>
                                <mu-list-item-sub-title>
                                    <span class="kc"></span>
                                    <p class="ostatus">
                                        <span>欢迎下次光临</span>
                                    </p>
                                </mu-list-item-sub-title>
                            </mu-list-item-content>
                        </mu-list-item>
                        <!--<mu-divider></mu-divider>-->
                        <!-- <div style="position: absolute;right: 0;bottom: 0;">
					    	<div class="saoma">
					        	<span>x1</span>
					        </div>
					    </div>-->
                    </div>
                </mu-list>
            </mu-paper>

            <!--交易关闭-->
            <mu-paper v-if='status == 5' :z-depth="1" class="demo-list-wrap">
                <mu-list textline="three-line">
                    <div class="li-box">
                        <mu-list-item avatar :ripple="false" button>
                            <mu-list-item-action>
                                <mu-avatar style="width: 1.4rem;height: 1.4rem;">
                                    <img src="../../../static/img/order/img_jiaoyiguanbi.png">
                                </mu-avatar>
                            </mu-list-item-action>
                            <mu-list-item-content class='dfh'>
                                <mu-list-item-title>交易关闭</mu-list-item-title>
                                <mu-list-item-sub-title>
                                    <span class="kc"></span>
                                    <p class="ostatus">
                                        <span>期待您下次光临</span>
                                    </p>
                                </mu-list-item-sub-title>
                            </mu-list-item-content>
                        </mu-list-item>
                        <!--<mu-divider></mu-divider>-->
                        <!-- <div style="position: absolute;right: 0;bottom: 0;">
					    	<div class="saoma">
					        	<span>x1</span>
					        </div>
					    </div>-->
                    </div>
                </mu-list>
            </mu-paper>

        </div>

        <div class="paddingbox">
            <mu-paper :z-depth="1" class="demo-list-wrap" style='margin-top:0.15rem;border-radius:15px;'>
                <mu-list textline="two-line" style="background:#fff;">
                    <mu-list-item avatar :ripple="false" button>
                        <mu-list-item-content>
                            <mu-list-item-title>收货人：{{datainfo.address}}</mu-list-item-title>
                            <mu-list-item-sub-title>
                                收货地址：{{datainfo.name}}
                            </mu-list-item-sub-title>
                        </mu-list-item-content>
                        <mu-list-item-action>
                            <mu-list-item-after-text class="mrdz">默认地址</mu-list-item-after-text>
                            <mu-list-item-after-text></mu-list-item-after-text>
                        </mu-list-item-action>
                    </mu-list-item>
                </mu-list>
            </mu-paper>

            <!--订单列表-->
            <div class="listbox" v-if="datainfo">

                <p class="ordernum">订单编号:{{datainfo.order_number}}</p>
                <mu-paper :z-depth="1" class="demo-list-wrap">
                    <mu-list textline="three-line">

                        <div v-for="(item,index) in datainfo.goodsInfo" :key="index" class="li-box">
                            <mu-list-item avatar :ripple="false" button>
                                <mu-list-item-action>
                                    <mu-avatar style="width: 1.4rem;height: 1.4rem;">
                                        <img :src="item.goods_photo" :onerror="onerrorimg">
                                    </mu-avatar>
                                </mu-list-item-action>
                                <mu-list-item-content>
                                    <mu-list-item-title>{{item.goods_title}}</mu-list-item-title>
                                    <mu-list-item-sub-title>
                                        <span class="kc">
                                            <!--库存{{item.inventory}}件-->
                                        </span>
                                        <p style="color: red;">
                                            ￥
                                            <span style="font-size: 0.5rem;">{{item.price}}</span>
                                            <!-- <span style="color: #ccc;text-decoration: line-through;">￥{{item.goods_original_price}}</span> -->
                                        </p>
                                    </mu-list-item-sub-title>
                                </mu-list-item-content>
                            </mu-list-item>
                            <mu-divider></mu-divider>
                            <div style="position: absolute;right: 0;bottom: 0;">
                                <div class="saoma">
                                    <span>x{{datainfo.count[index]}}</span>
                                </div>
                            </div>
                        </div>
                    </mu-list>

                    <mu-list class="infoboxbut">
                        <mu-list-item button :ripple="false">
                            <mu-list-item-title>商品总价</mu-list-item-title>
                            <mu-list-item-action>
                                <span>￥{{datainfo.sum_price}}</span>
                            </mu-list-item-action>
                        </mu-list-item>
                        <mu-list-item button :ripple="false">
                            <mu-list-item-title>优惠券优惠</mu-list-item-title>
                            <mu-list-item-action>
                                <span>￥{{datainfo.red_packet_price}}</span>
                            </mu-list-item-action>
                        </mu-list-item>
                        <mu-list-item button :ripple="false">
                            <mu-list-item-title>运费（满300免运费）</mu-list-item-title>
                            <mu-list-item-action>
                                <span>￥{{datainfo.distribution_fee}}</span>
                            </mu-list-item-action>
                        </mu-list-item>
                        <mu-list-item button :ripple="false" class='blodtext'>
                            <mu-list-item-title>订单总额</mu-list-item-title>
                            <mu-list-item-action>
                                <span>￥{{datainfo.sum_price}}</span>
                            </mu-list-item-action>
                        </mu-list-item>
                        <!--等待付款-->
                        <mu-list-item v-if='status == 1' button :ripple="false" class='blodtext'>
                            <mu-list-item-title>需付款</mu-list-item-title>
                            <mu-list-item-action>
                                <span style="color: #f95151;font-weight: bold;font-size:0.32rem;">￥{{datainfo.real_pay_price}}</span>
                            </mu-list-item-action>
                        </mu-list-item>

                        <!--已付款或者交易完成-->
                        <mu-list-item v-if='status == 2 || status == 4' button :ripple="false" class='blodtext'>
                            <mu-list-item-title>实付款</mu-list-item-title>
                            <mu-list-item-action>
                                <span style="color: #f95151;font-weight: bold;font-size:0.32rem;">￥{{datainfo.real_pay_price}}</span>
                            </mu-list-item-action>
                        </mu-list-item>
                        <div id="realheight"></div>
                    </mu-list>
                </mu-paper>
            </div>

            <!--已发货-->
            <div class="addToCar" v-if='status == 3'>
                <mu-list class="carbut">
                    <mu-list-item avatar button :ripple="false">
                        <div class="pricebox">
                            <ul>
                                <!-- <li class="payorder">查看物流</li> -->
                                <li class="cancelorder" @click="confirmOrder">确认收货</li>
                            </ul>
                        </div>
                    </mu-list-item>
                </mu-list>
            </div>

            <!--待付款-->
            <div class="addToCar" v-if='status == 1'>
                <mu-list class="carbut">
                    <mu-list-item avatar button :ripple="false">
                        <div class="pricebox">
                            <ul>
                                <li class="payorder" @click="WXPay">立即支付</li>
                                <li class="cancelorder" @click="cancelorder">取消订单</li>
                            </ul>
                        </div>
                    </mu-list-item>
                </mu-list>
            </div>

            <!--交易取消-->
            <div class="addToCar" v-if='status == 5'>
                <mu-list class="carbut">
                    <mu-list-item avatar button :ripple="false">
                        <div class="pricebox">
                            <ul>
                                <li class="cancelorder" @click="deleteorder">删除订单</li>
                            </ul>
                        </div>
                    </mu-list-item>
                </mu-list>
            </div>
        </div>

    </div>
</template>

<script>
import BackBar from "../common/BackBar.vue";
import { WXPay } from "../../common/common.js";
import { getDress, cancelOrder, deleteOrder , confirmOrder } from "../../http/http.js";
import { disableShare } from "../../common/disableShare.js";

export default {
  data() {
    return {
      dTitle: "确认订单",
      vactiveStep: 0,
      onerrorimg: this.$store.state.onerrorimg,
      datainfo: null,
      page: 1,
      limit: 99, //当前页面分页条数
      status: null, //订单状态
      dress: null, //默认地址
      list: []
    };
  },
  computed: {
    vfinished() {
      return this.vactiveStep > 2;
    }
  },
  activated() {
    let status = this.$route.query.status;
    this.status = status;
    this.datainfo = JSON.parse(this.$route.query.data);
    this.datainfo.count = eval("(" + this.datainfo.count + ")");
    console.log("***********************");
    console.log(this.datainfo);

    /**获取收货地址*/
    /* 	getDress(this.limit , this.page).then(res => {
				let data = res.data.data.data;
				for(let item in data){
					if(data[item].is_default == 1){
						this.dress = data[item]
					}
				}
    		}); */
  },
  mounted() {
    document.title = "订单状态";
    //disableShare();
  },
  components: {
    BackBar
  },
  methods: {
    /**支付*/
    WXPay() {
      WXPay(this.datainfo.id);
    },
    /**取消订单*/
    cancelorder() {
      let id = this.datainfo.id;
      let redPacketId = this.datainfo.red_packet_id;
      alert(id);
      alert(redPacketId);
      cancelOrder(id, redPacketId).then(res => {
       this.$store.commit("setShowText", '已取消订单');
        this.$store.commit("showInfo");
        console.log(res);
      });
    },

    /**删除订单*/
    deleteorder() {
      let id = this.datainfo.id;
      let redPacketId = this.datainfo.red_packet_id;
      alert(id);
      alert(redPacketId);
      deleteOrder(id, redPacketId).then(res => {
        this.$store.commit("setShowText", '已删除订单');
        this.$store.commit("showInfo");
        console.log(res);
      });
    },
     /**确认收货*/
    confirmOrder(){
        confirmOrder(this.datainfo.id).then(res=>{
            this.$store.commit("setShowText", '已确认订单');
            this.$store.commit("showInfo");
        })
    },
    /*查看物流详情*/
    goExpress() {
      this.$router.push("/express");
    },
    goAddDress() {
      this.$router.push("/address");
    },
    toPay() {
      //this.$router.push('/paysuccessful')
      this.$router.push("/payfailure");
    },
    vhandleNext() {
      this.vactiveStep++;
    },
    vhandlePrev() {
      this.vactiveStep--;
    },
    vreset() {
      this.vactiveStep = 0;
    }
  }
};
</script>

<style scoped>
.paddingbox {
  padding: 0 14px 0 14px;
  padding-top: -30px;
  position: relative;
  z-index: 9;
}
.status {
  padding-top: 1rem;
}
.status .mu-paper {
  background: url(../../../static/img/order/img_zhuangtai_bj.png) no-repeat
    center;
  background-size: 100%;
}
.orderstatusbox {
  margin-bottom: 1rem;
}
.orderstatusbox .title {
  height: 1.03rem;
  line-height: 1.03rem;
  font-size: 0.28rem;
  font-weight: bold;
  color: #f95151;
  padding-left: 10px;
}
.shdz {
  background: #fff;
}
.orderstatusbox .shdz img {
  width: 0.33rem;
  height: 0.33rem;
}

.total {
  height: 0.8rem;
  line-height: 0.8rem;
  background: #fff;
  margin-top: 0.15rem;
}
.total span {
  float: right;
}
.xj {
  padding-right: 0.3rem;
}

.orderstatusbox .listbox {
  width: 100%;
  margin-top: 0.15rem;
}
.li-box {
  position: relative;
}
.saoma {
  float: right;
  padding: 0.2rem 0;
  border-radius: 0.2rem;
  margin-right: 0.3rem;
}
.minus {
  background: #c3c3c3;
}
.saoma span {
  margin-left: 0.15rem;
}
.mu-list-three-line .mu-item-action .mu-avatar {
  margin-top: 0;
  margin-right: 0.09rem;
}
.mu-avatar img {
  border-radius: 0.2rem;
}
.li-box .mu-item-title {
  font-size: 0.24rem;
  font-weight: bold;
}
.mu-list {
  padding: 8px 0 0 0;
}
.mu-elevation-1 {
  box-shadow: none;
}
.clear {
  clear: both;
}
.mu-drawer {
  position: fixed;
  top: 4.3rem;
}

.addToCar {
  position: fixed;
  bottom: 0;
  width: 100%;
  left: 0;
}
.pricebox {
  width: 100%;
  background: #fff;
  left: 0;
  position: absolute;
  height: 100%;
}
.pricebox li {
  font-size: 0.32rem;
  font-weight: bold;
  border-radius: 0.4rem;
  padding: 0.06rem 0.3rem;
  float: right;
  margin-top: 0.13rem;
  margin-right: 20px;
}
.pricebox .cancelorder {
  border: 1px solid #c6c6c6;
  color: #666;
}
.pricebox .payorder {
  border: 1px solid #f95151;
  color: #f95151;
}

.carprice {
  position: absolute;
  left: 0.8rem;
  font-size: 0.26rem !important;
  color: #666;
  z-index: 9;
}
.settlement {
  position: absolute;
  right: 0px;
  background: #ffe253;
  width: 33%;
  height: 100%;
  line-height: 45px;
}
.carlistbox {
  padding: 0.15rem 0.3rem !important;
}
.carlistbox > ul {
  overflow: hidden;
}

.mrdz {
  font-size: 12px;
  padding: 2px 3px;
  border: 1px solid #f95151;
  color: #f95151;
  border-radius: 15px;
}
.kc {
  color: #a9a9a9;
  font-size: 0.2rem;
  min-height: 0.14rem;
  display: block;
}
.ostatus {
  color: #fff;
  font-size: 12px;
}
.status .mu-item-title {
  margin-top: 0.2rem;
  font-size: 0.28rem;
  color: #fff;
}
.status .mu-avatar {
  background: initial;
}
.status li {
  overflow: hidden;
}
.statustime {
  font-size: 0.22rem;
  color: #fff;
}
.dfh {
  height: 100%;
}
.ckxq {
  position: absolute;
  right: 0;
  bottom: 0;
}
.wuliubox {
  background: #fff;
  margin-top: 0.15rem;
}
.mu-item-sub-title {
  text-overflow: inherit;
  white-space: normal;
  word-wrap: initial;
}
.mu-step-label,
.mu-step-content-inner p {
  text-align: left !important;
}
.ordernum {
  padding: 0.2rem;
  background: #fff;
  text-align: left;
  border-bottom: 1px solid rgba(0, 0, 0, 0.12);
  font-weight: bold;
}
.infoboxbut li {
  padding: 0.075rem 0;
}
.blodtext {
  font-weight: bold;
}
.infoboxbut li:last-child {
  border-top: 1px solid #e0e0e0;
}
</style>