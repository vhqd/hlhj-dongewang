<template>
	<div class="chuanchuan">
		<BackBar dTitle='热销榜'></BackBar>
		<!--菜品列表-->
		<div class="cailistbox">
			<mu-paper :z-depth="1" class="demo-list-wrap">
				<mu-list textline="three-line">

					<div v-for="(item,index) in list" :key="index" class="li-box">
						<router-link :to="{path:'/detail',query: {id: item.id}}">
							<mu-list-item avatar :ripple="false" button>
								<mu-list-item-action style='position: relative;'>
									<mu-avatar style="min-width: 1.4rem;height: 1.4rem;margin-top:0;">
										<img :src="host+item.goods_photo" :onerror='onerrorimg'>
									</mu-avatar>
									<img v-show="index == 0" class="listimg" src="../../../static/img/hot/img_1.png" />
									<img v-show="index == 1" class="listimg" src="../../../static/img/hot/img_2.png" />
									<img v-show="index == 2" class="listimg" src="../../../static/img/hot/img_3.png" />
									<img v-show="index == 3" class="listimg" src="../../../static/img/hot/img_4.png" />
								</mu-list-item-action>
								<mu-list-item-content>
									<mu-list-item-title>{{item.goods_title}}</mu-list-item-title>
									<span style="color: #666;font-size: 0.2rem;padding-left:.2rem;">{{item.specification}}</span>
									<mu-list-item-sub-title>
										<p style="color: red;">
											<span v-show="!isbind" style="color: #a9a9a9;">绑定手机号才能查看价格</span>
											<span v-show="isbind">￥
												<span style="font-size: .28rem;">{{item.price}}</span>
											</span>
										</p>
									</mu-list-item-sub-title>
								</mu-list-item-content>
							</mu-list-item>
						</router-link>
						<mu-divider></mu-divider>
						<div style="position: absolute;right: 0.3rem;bottom: 0.12rem;min-width:80px;">
							<div class="saoma">
								<!-- <span class="minus mpsytl" @click="minus(item)" v-if="item.num != 0">-</span>
				        	<span>{{item.num}}</span>
				        	<span class="plus mpsytl" @click="plus(item)">+</span> -->
								<span class="minus" @click="minus(item)" v-if="item.count != 0"><img src="../../../static/img/ic_jian.png" alt="" style="width:.5rem;height:.5rem;"></span>
								<span v-show="item.count != 0">{{item.count}}</span>
								<span class="plus" @click="plus(item)"><img src="../../../static/img/ic_jia.png" alt="" style="width:.5rem;height:.5rem;"></span>
							</div>
						</div>
					</div>

				</mu-list>
			</mu-paper>
      <div id="realheight"></div>
		</div>

		<div class="addToCar">
			<mu-list class="carbut">
				<mu-list-item avatar button :ripple="false">
					<div class="pricecarbox">
						<div class="carprice" @click="goShopCar">
							<div class="carimgbox">
								<img src="../../../static/img/car/car.png" />
								<span class="carnum">{{carnum}}</span>
							</div>
						</div>
						<div style="position: relative;">合计：
							<span style="color: red;">￥{{allPrice}}</span>
							<span class="qigou">{{qigou}}元起购</span>
						</div>
					</div>
					<div v-if="allPrice < qigou" class="huise settlement">
						去结算
					</div>
						<div v-else class="settlement" @click="settlement">
						去结算
					</div>
				</mu-list-item>
			</mu-list>
		</div>
<mu-dialog title="温馨提示" width="360" :open.sync="openwins">
        <span class="cancelbox" @click="cancel"><img src="../../../static/img/ic_Shut .png" /></span>
        绑定手机才可以下单呦~<br />
        <mu-button slot="actions" flat color="primary" @click="sure">确定</mu-button>
        <mu-button slot="actions" flat color="secondary" @click="cancel">取消</mu-button>
      </mu-dialog>
	</div>
</template>

<script>
import { mapState } from "vuex";
import BackBar from "../common/BackBar.vue";
import { getHotList , EditCarShop } from "../../http/http.js";
import { jiancar, setOfenBuyData , jianHotcar  } from "../../common/common.js";
import QS from 'qs'
  import { share } from '../../common/share.js';

export default {
  data() {
    return {
      carnum: 0,
	  allPrice: 0,
	  host:this.$store.state.host,
	  qigou: this.$store.state.qigou,
    onerrorimg:this.$store.state.onerrorimg,
     openwins:false,
      list: []
    };
  },
  components: {
    BackBar
  },
  mounted() {
    this.carnum = 0
	  this.allPrice = 0
    this.getHotLists()
    
    let invate_code = localStorage.invate_code
    if (invate_code && invate_code!= '') {
       share(this.$store.state.shareurl,localStorage.invate_code,this.$store.state.shareimg)
    }
  },
  activated(){
    document.title = '热销榜'
	  
  },
  methods: {
	/**获取热销榜列表*/
	getHotLists(){
		getHotList().then(res => {
			let data = res.data.info
			for(let item in data){
				data[item].count = 0
				data[item].type = 1
			}
			console.log(data);
			this.list = data
		})
	},
    /*结算*/
    settlement() {
      //用选中商品总价判断是否选择商品
      if (this.allPrice > 0) {
        let data = this.list;
        let datas = [];
        console.log(2222222222222222222222222);
        for(let item in data){
        if(data[item].count > 0){
          datas.push(data[item]);
        }
        }
        console.log(datas);
        
        this.$router.push({ path: "/order" ,query:{list:JSON.stringify(datas)}});
      } 
    },
    /**点击底部购物车图标到购物车*/
    goShopCar(){
      this.$router.push('/car')
    },
    /*减少数量值*/
    minus(item) {
      let amount = item.count;
      if (amount > 0) {
        item.count = amount - 1;
        this.carnum = this.carnum - 1;
        this.allPrice = this.allPrice -parseFloat(item.price);
      } else {
        item.count = 0;
      }
      let data = {
          specification_id:item.specification_id,
          goods_id: item.id,
          single_price: item.price,
          count: 1,
          isadd:0
        };
      console.log('减少购物车');
      console.log(data);
      jianHotcar(item.id , QS.stringify(data))
    },
    /*增加数量值*/
    plus(item) {
      let isbind = localStorage.isbind
      if(isbind != 1){
        this.openwins = true
      }else{
        this.carnum = this.carnum + 1;
        this.allPrice = this.allPrice +parseFloat(item.price);
        let amount = item.count;
      item.count = amount + 1;
        let list =  [{
          goods_id: [],
          single_price: [],
          count: [],
          sum_price: 0,
          specification_id: []
        }]
         setOfenBuyData(item, list , {});
      }
    },
    sure(){
      this.openwins = false
      this.$router.push('/phone')
    },
    cancel(){
      this.openwins = false
    }
  },
  computed: {
    ...mapState({
      // mapState相当于映射
      isbind: "isbind"
    })
  }
};
</script>

<style scoped>
.adimg img {
  height: 2.06rem;
  width: 100%;
}
.cailistbox {
  padding-top: 1rem;
}

.mu-list {
  padding: 8px 0 0 0; /*margin-bottom: 1rem;*/
}
.li-box .mu-item-title {
  font-size: 0.24rem;
}
.mu-item-action {
  padding-top: 0.3rem;
}
.mu-avatar img {
  border-radius: initial;
}
.li-box {
  position: relative;
}
.mu-item-sub-title,
.mu-item-title {
  padding-left: 0.2rem;
}
.mu-item-content {
  max-width: 75% !important;
}
.mu-item-title {
  height: auto;
  overflow: initial;
  text-overflow: initial;
  white-space: initial;
}
.mu-list {
  padding: 0;
}
.li-box li {
  padding: 0;
}
.mu-item-title {
  line-height: 0.36rem;
}

.type2list .mu-item-content {
  max-width: 85% !important;
}
.type2list li {
  padding: 0;
}

.saoma img {
  width: .6rem;
  height: .6rem;
}

.addToCar {
  position: fixed;
  bottom: 0;
  width: 100%;
  z-index: 2;
}
.pricecarbox {
  width: 67%;
  background: #fff;
  left: 0;
  position: absolute;
  height: 100%;
}
.pricecarbox > div {
  line-height: 32px;
  text-align: right;
  padding-right: 0.3rem;
  font-weight: bold;
  font-size: 0.32rem;
}
.carprice {
  position: absolute;
  left: 0.8rem;
  font-size: 0.26rem !important;
  color: #666;
}
.settlement {
  position: absolute;
  right: 0px;
  background: #f24c4c;
  width: 33%;
  height: 100%;
  line-height: 45px;
  color: #fff;
}
.carlistbox {
  padding: 0.15rem 0.3rem !important;
}
.carlistbox > ul {
  overflow: hidden;
}
.carimgbox img {
  width: 0.47rem;
  height: 0.47rem;
}
.addToCar .mu-list {
  overflow: initial;
}
.carimgbox {
  width: 1rem;
  height: 1rem;
  background: #fff;
  border-radius: 100%;
  margin-top: -15px;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0px 1px 1px rgba(0, 0, 0, 0.2);
}
.carnum {
  position: absolute;
  top: -0.1rem;
  left: 0.5rem;
  color: #fff;
  background: red;
  line-height: 0.25rem;
  border-radius: 0.2rem;
  min-width: 0.25rem;
  height: 0.25rem;
  padding: 0 5px;
  text-align: center;
}
.qigou {
  position: absolute;
  right: 0.3rem;
  bottom: -0.36rem;
  font-size: 0.22rem;
  color: #999;
}
.opentab {
  width: 100%;
  background: #fff;
  border-bottom: 1px solid #e0e0e0;
  padding: 0.1rem;
}

.listimg {
  position: absolute;
  top: 0;
  left: 0;
  width: 0.52rem;
  height: 0.56rem;
}
</style>