<template>
	<div class = "pos">
		<el-row>
			<el-col :span = "7" class = "pos-order" id = "order-list">
				<el-tabs>
					<el-tab-pane label = "点餐">
						<el-table :data = "tableData" border style = "width : 100%;">
							<el-table-column prop = "goodsName" label = "商品名称">
							</el-table-column>
								<el-table-column prop = "count" label = "量" width = "50">
							</el-table-column>
								<el-table-column prop = "price" label = "价格" width = "70">
							</el-table-column>
								<el-table-column label = "操作" width = "100" fixed = "right">
								<template scope = "scope">
									<el-button type = "text" size = "small" @click = "delSingleGoods(scope.row)">删除</el-button>
									<el-button type = "text" size = "small" @click = "addOrderList(scope.row)">增加</el-button>
								</template>
							</el-table-column>
						</el-table>
						<div class = "totalDiv">
							<small>数量:</small> {{tatalCount}}
							<small>金额</small> {{tatalMoney}}元
						</div>
						<div class = "divBtn">
							<el-button type = "warning">挂单</el-button>
							<el-button type = "danger" @click = "delAllGoods">删除</el-button>
							<el-button type = "success" @click = "checkOut">结账</el-button>
						</div>
					</el-tab-pane>
					<el-tab-pane label = "挂单">
						挂单
					</el-tab-pane>
					<el-tab-pane label = "外卖">
						外卖
					</el-tab-pane>
				</el-tabs>
			</el-col>
			<el-col :span = "17">
				<div>
					<div class = "title">常用商品</div>
					<ul >
						<li v-for = "goods in oftenGoods" class = "ofen-goods-list" @click = "addOrderList(goods)">
							<span>{{goods.goodsName}}</span>
							<span class= "o-price">{{goods.price}}</span>
						</li>
					</ul>
				</div>
				<div class = "goods-type">
					<el-tabs>
						<el-tab-pane label = "汉堡">
							<ul class = "cookList">
								<li v-for="goods in type0Goods"  @click = "addOrderList(goods)" >
									<span class="foodImg">
										<img :src="goods.goodsImg" alt="">
									</span>
									<span class = "foodName">{{goods.goodsName}}</span>
									<span class = "foodPrice">¥{{goods.price}}</span>
								</li>
							</ul>
						</el-tab-pane>
						<el-tab-pane label = "小食">
							<ul class = "cookList">
								<li v-for="goods in type1Goods"  @click = "addOrderList(goods)">
									<span class="foodImg">
										<img :src="goods.goodsImg" alt="">
									</span>
									<span class = "foodName">{{goods.goodsName}}</span>
									<span class = "foodPrice">¥{{goods.price}}</span>
								</li>
							</ul>
						</el-tab-pane>
						<el-tab-pane label = "饮料">
							<ul class = "cookList">
								<li v-for="goods in type2Goods"  @click = "addOrderList(goods)" >
									<span class="foodImg">
										<img :src="goods.goodsImg" alt="">
									</span>
									<span class = "foodName">{{goods.goodsName}}</span>
									<span class = "foodPrice">¥{{goods.price}}</span>
								</li>
							</ul>
						</el-tab-pane>
						<el-tab-pane label = "套餐">
							<ul class = "cookList">
								<li v-for="goods in type3Goods"  @click = "addOrderList(goods)" >
									<span class="foodImg">
										<img :src="goods.goodsImg" alt="">
									</span>
									<span class = "foodName">{{goods.goodsName}}</span>
									<span class = "foodPrice">¥{{goods.price}}</span>
								</li>
							</ul>
						</el-tab-pane>
					</el-tabs>
				</div>
			</el-col>
		</el-row>
	</div>
</template>
<script>
import axios from 'axios'
	export default {
		name : "pos",
		data(){
			return {
				tableData: [],
		       oftenGoods : [],
	           type0Goods:[],
	           type1Goods:[],
	           type2Goods:[],
	           type3Goods:[],
	           tatalMoney:0,
	           tatalCount :0
			}
		},
		created : function () {
			// 读取常用商品列表
			axios.get('http://jspang.com/DemoApi/oftenGoods.php')
			.then(response =>{
				// console.log(response);
				this.oftenGoods = response.data
			})
			.catch(error =>{
				// console.log(error);
				alert("网络错误")
			})
			// 读取分类商品列表
			axios.get('http://jspang.com/DemoApi/typeGoods.php')
			.then(response=>{
				this.type0Goods = response.data[0];
                this.type1Goods = response.data[1];
                this.type2Goods = response.data[2];
                this.type3Goods = response.data[3];
			})
			.catch(error => {
				alert("网络错误")
			})
		},
		mounted : function () {
			var orderHeight = document.body.clientHeight;
			document.getElementById("order-list").style.height = orderHeight + "px"
		},
		methods : {
			addOrderList(goods){
				//商品是否存在于订单列表中
				let isHave = false;
				this.tatalCount = 0;
				this.tatalMoney = 0;
				for(var i = 0; i < this.tableData.length; i++){
					if(this.tableData[i].goodsId == goods.goodsId){
						isHave = true;
					}
				}
				//根据判断的值编写业务逻辑
				if(isHave) {
					let arr = this.tableData.filter(o=>o.goodsId == goods.goodsId)
						arr[0].count++;
				}else{
					let newGoods = {goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1}
					this.tableData.push(newGoods)
				}
				this.getAllMoney()
			},
			// 删除单个商品
			delSingleGoods(goods){
				this.tableData = this.tableData.filter(o=>o.goodsId != goods.goodsId);
				this.getAllMoney()
			},
			// 全部删除
			delAllGoods(){
				this.tableData = [];
				this.tatalCount = 0;
				this.tatalMoney = 0;
			},
			// 汇总数量金额
			getAllMoney(){
				this.tatalCount = 0;
				this.tatalMoney = 0;
				if(this.tableData){
					//计算汇总金额和数量
					this.tableData.forEach((element)=>{
						this.tatalCount+=element.count;
						this.tatalMoney = this.tatalMoney + (element.price*element.count)
					})
				}
			},
			checkOut(){
				// 模拟结账
				if(this.tatalCount){
					this.tableData = [];
					this.tatalCount = 0;
					this.tatalMoney = 0;
					this.$message({
						message : "感谢光临",
						type : "success"
					})
				}else{
					this.$message.error("不能空结")
				}
			}
		}
	}
</script>
<style>
	.pos-order{
		background:#999;
		border-right: 1px solid #fff;
		height:100%;
	}
	.divBtn{
		margin-top:10px;
	}
	.title{
		height:20px;
		border-bottom:1px solid #d3dce6;
		padding:10px;
		text-align: left;
		background: #f9fafc;
	}
	.ofen-goods-list{
		list-style:none;
		float:left;
		border:1px solid #e5e9f2;
		padding: 10px;
		margin:10px;
		background: #fff;
	}
	.o-price{
		color:#58bcff;
	}
	.goods-type{
		clear : both;
	}
	.cookList li{
       list-style: none;
       width:23%;
       border:1px solid #E5E9F2;
       height: auot;
       overflow: hidden;
       background-color:#fff;
       padding: 2px;
       float:left;
       margin: 2px;
 
   }
  .cookList li{
       list-style: none;
       width:23%;
       border:1px solid #E5E9F2;
       height: auot;
       overflow: hidden;
       background-color:#fff;
       padding: 2px;
       float:left;
       margin: 2px;
 
   }
   .cookList li span{
        display: block;
        float:left;
   }
   .foodImg{
       width: 40%;
   }
   img{
   	width:100%;
   }
   .foodName{
       font-size: 17px;
       padding-left: 10px;
       color:brown;
   }
   .foodPrice{
       font-size: 16px;
       padding-left: 10px;
       padding-top:10px;
   }
   .totalDiv{
   	background: #fff;
   	padding:10px;
   	border-bottom: 1px solid pink;
   }
</style>