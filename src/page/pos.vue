<template>
  <div class="pos">
    <div>
      <el-row>
        <el-col :span="7" class="pos-order" id="order-list">
          <el-tabs>
            <el-tab-pane label="点餐">
              <el-table border style="width:100%" :data="tableData" show-summary>
                <el-table-column prop="goodsName" label="商品"></el-table-column>
                <el-table-column prop="count" label="数量"></el-table-column>
                <el-table-column prop="price" label="金额"></el-table-column>
                <el-table-column prop="goodsName" label="操作" fixed="right">
                  <template scope="scope">
                    <el-button type="text" size="small" @click="delOrderListrow(scope.row)">删除</el-button>
                    <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                  </template>
                </el-table-column>
              </el-table>
            </el-tab-pane>
            <el-tab-pane label="挂单">
              <el-table border style="width:100%" :data="tableData2" show-summary>
                <el-table-column prop="goodsName" label="商品"></el-table-column>
                <el-table-column prop="count" label="数量"></el-table-column>
                <el-table-column prop="price" label="金额"></el-table-column>
                <el-table-column prop="goodsName" label="操作" fixed="right">
                  <template scope="scope">
                    <el-button type="text" size="small">删除</el-button>
                    <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                  </template>
                </el-table-column>
              </el-table>
            </el-tab-pane>
            <el-tab-pane label="外卖">
              <el-table border style="width:100%" :data="tableData3" show-summary>
                <el-table-column prop="goodsName" label="商品"></el-table-column>
                <el-table-column prop="count" label="数量"></el-table-column>
                <el-table-column prop="price" label="金额"></el-table-column>
                <el-table-column prop="goodsName" label="操作" fixed="right">
                  <template scope="scope">
                    <el-button type="text" size="small">删除</el-button>
                    <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                  </template>
                </el-table-column>
              </el-table>
            </el-tab-pane>
          </el-tabs>
          <div class="btn-three">
            <el-button type="warning" @click="entryOrders()">挂单</el-button>
            <el-button type="danger" @click="delOrderList()">删除</el-button>
            <el-button type="success" @click="checkout()">结账</el-button>
          </div>
        </el-col>
        <el-col :span="17">
          <div class="often-goods">
            <div class="title">常用商品</div>
            <div class="often-goods-list">
              <ul>
                <li v-for="goods in oftenGoods" style="width:120px;height:42px" :key="goods.index" @click="addOrderList(goods)">
                  <span class="Length-font" id="LengthFont">{{goods.title}}</span>
                  <span class="o-price">￥{{goods.price}}元</span>
                </li>
              </ul>
            </div>
          </div>
          <div class="goods-type">
            <el-tabs>
              <el-tab-pane label="汉堡">
                <ul class="cookList">
                  <li v-for="goods in type1Goods" :key="goods.index" @click="addOrderList(goods)">
                    <span class="foodImg">
                      <img :src="goods.image" width="100%" height="60px" />
                      <!-- <img :src="goods.goodsImg" width="100%" /> -->
                    </span>
                    <span class="foodName Length-font">{{goods.title}}</span>
                    <span class="foodPrice">￥{{goods.price}}元</span>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="小食">
                <ul class="cookList">
                  <li v-for="goods in type2Goods" :key="goods.index" @click="addOrderList(goods)">
                    <span class="foodImg">
                      <img :src="goods.image" width="100%" height="60px" />
                      <!-- <img :src="goods.goodsImg" width="100%" /> -->
                    </span>
                    <span class="foodName Length-font">{{goods.title}}</span>
                    <span class="foodPrice">￥{{goods.price}}元</span>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="饮料">
                <ul class="cookList">
                  <li v-for="goods in type3Goods" :key="goods.index" @click="addOrderList(goods)">
                    <span class="foodImg">
                      <img :src="goods.image" width="100%" height="60px" />
                      <!-- <img :src="goods.goodsImg" width="100%" /> -->
                    </span>
                    <span class="foodName Length-font">{{goods.title}}</span>
                    <span class="foodPrice">￥{{goods.price}}元</span>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="套餐">
                <ul class="cookList">
                  <li v-for="goods in type4Goods" :key="goods.index" @click="addOrderList(goods)">
                    <span class="foodImg">
                      <img :src="goods.image" width="100%" height="60px" />
                      <!-- <img :src="goods.goodsImg" width="100%" /> -->
                    </span>
                    <span class="foodName Length-font">{{goods.title}}</span>
                    <span class="foodPrice">￥{{goods.price}}元</span>
                  </li>
                </ul>
              </el-tab-pane>
            </el-tabs>
          </div>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Pos",
  data() {
    return {
      //存点餐数据
      tableData: [],
      //存挂单数据
      tableData2: [],
      //存外卖数据
      tableData3: [],
      oftenGoods: [],
      type1Goods: [],
      type2Goods: [],
      type3Goods: [],
      type4Goods: []
    };
  },
  created: function() {
    axios
      .get("https://www.xiongmaoyouxuan.com/api/tab/10?start=0")
      .then(response => {
        //console.log(response);
        this.oftenGoods = response.data.data.items.list;
      })
      .catch(error => {
        //console.log(error);
        alert("网络错误，不能访问");
      });
    axios
      .get("https://www.xiongmaoyouxuan.com/api/column/3464/items?start=0")
      .then(response => {
        //console.log(response);
        this.type1Goods = response.data.data.list;
      })
      .catch(error => {
        //console.log(error);
        alert("网络错误，不能访问");
      });
    axios
      .get("https://www.xiongmaoyouxuan.com/api/column/3464/items?start=20")
      .then(response => {
        this.type2Goods = response.data.data.list;
      })
      .catch(response => {
        alert("网络错误，不能访问");
      });
    axios
      .get("https://www.xiongmaoyouxuan.com/api/column/3464/items?start=40")
      .then(response => {
        this.type3Goods = response.data.data.list;
      })
      .catch(response => {
        alert("网络错误，不能访问");
      });
    axios
      .get("https://www.xiongmaoyouxuan.com/api/column/3464/items?start=60")
      .then(response => {
        this.type4Goods = response.data.data.list;
      })
      .catch(response => {
        alert("网络错误，不能访问");
      });
  },
  methods: {
    //添加订单列表的方法
    addOrderList(goods) {
      let isHave = false;
      //判断是否这个商品已经存在于订单中
      for (let i = 0; i < this.tableData.length; i++) {
        console.log(this.tableData[i].id);
        if (this.tableData[i].id == goods.id) {
          isHave = true;
        }
      }
      for (let i = 0; i < this.tableData2.length; i++) {
        if (this.tableData2[i].id == goods.id) {
          isHave = true;
        }
      }
      //根据isHave的值判断订单中列表中是否已经有此商品
      if (isHave) {
        //存在就进行数量的添加
        let arr = this.tableData.filter(o => o.id == goods.id);
        arr[0].count++;
        arr[0].price += goods.price;
      } else {
        //不存在放进数组里面
        let newGoods = {
          id: goods.id,
          goodsName: goods.title,
          price: goods.price,
          count: 1
        };
        this.tableData.push(newGoods);
      }
    },
    //删除单个商品
    delOrderListrow(goods) {
      this.tableData = this.tableData.filter(o => o.id != goods.id);
      console.log(goods);
    },
    //挂单
    entryOrders(goods) {
     
      console.log(this.tableData);
    },
    //删除全部商品
    delOrderList() {
      this.tableData = [];
    },
    //结账
    checkout() {
      if (this.tableData.length != 0) {
        this.tableData = [];
        this.$message({
          message: "结账成功，感谢你又为店里出了一份力!",
          type: "success"
        });
      } else {
        this.$message.error("不能空结,老板了解你急切的心情！");
      }
    }
  },
  mounted: function() {
    var orderHeiht = document.body.clientHeight;
    document.getElementById("order-list").style.height = orderHeiht + "px";
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pos-order {
  background-color: #f9fafc;
  border-right: 1px solid #c0ccda;
}
.btn-three {
  display: flex;
  justify-content: space-around;
}
.title {
  height: 20px;
  border-bottom: 1px solid #d3dce6;
  background-color: #f9fafc;
  padding: 10px;
}
.often-goods-list ul li {
  list-style: none;
  float: left;
  border: 1px solid #e5e9f2;
  padding: 10px;
  margin: 15px;
  background-color: #fff;
}
.o-price {
  color: #58b7ff;
}
.goods-type {
  clear: both;
}
.cookList li {
  list-style: none;
  width: 18%;
  border: 1px solid #e5e9f2;
  height: auot;
  overflow: hidden;
  background-color: #fff;
  padding: 2px;
  float: left;
  margin: 5px;
}
.cookList li span {
  display: block;
  float: left;
}
.foodImg {
  width: 40%;
}
.foodName {
  font-size: 18px;
  padding-left: 10px;
  color: brown;
}
.foodPrice {
  font-size: 16px;
  padding-left: 10px;
  padding-top: 10px;
}
.Length-font {
  display: block;
  width: 120px;
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
}
</style>