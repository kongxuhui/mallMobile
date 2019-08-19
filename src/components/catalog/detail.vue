<template>
  <div class="detail">
    <div class="detailContent">
      <div class="banner">
        <router-link to=""><span onclick="window.history.go(-1)" class="el-icon-arrow-left"></span></router-link>
      <mt-swipe :auto="0">
        <mt-swipe-item v-for="(item,index) in goodDetail.proList" :key="index">
            <img :src="'http://47.97.80.89:8082' + item.url" alt="">
        </mt-swipe-item>
      </mt-swipe>
      </div>
      <div class="introduce">
        <p>{{ goodDetail.name }}</p>
      </div>
      <div class="price">
        <p class="now-price">￥{{ goodDetail.price }}</p>
        <!-- <p class="old-price">原价：<del>￥{{ detailData[2] }}</del></p> -->
        <!-- <div class="delivery">
          <p class="delivery-left">包邮</p>
          <p class="delivery-right">上海</p>
        </div> -->
      </div>

      <div class="promise"  @click="showServer">
        <div class="promise-container">
          <!-- <p class="promise1"><span class="el-icon-circle-check"></span>运费险</p> -->
          <p class="promise2"><span class="el-icon-circle-check"></span>单位：{{goodDetail.unit}}</p>
          <p class="promise3"><span class="el-icon-circle-check"></span>库存:  {{goodDetail.count}}</p>
        </div>
        <!-- <p class="arrow-right"><span class="el-icon-arrow-right"></span></p> -->
      </div>
      <div class="sale" @click="showSale">
        <p class="sale-left">本店活动</p>
        <!-- <p class="arrow-right"><span class="el-icon-arrow-right"></span></p> -->
      </div>
      <ul class="product_ul">
        <li v-for="(item, index) in goods" @click="goDetail(item)" :key="index">
          <router-link :to="{ path:'/catalog/detail', query:{gid:item.id}}"><img class="animated zoomIn" :src="item.thumb" alt=""></router-link>
          <p>{{item.title}}</p>
          <p class="yuan">￥{{item.price}}</p>
          <!-- <p class="car" @click="addCar(item)"></p> -->
        </li>
      </ul>
    </div>
    <div class="detail-footer">
      <!-- <div class="left">
        <div class="store">
          <img src="./store.png" alt="">
          <p @click="store">收藏</p>
        </div>
      </div> -->
      <div class="right">
        <button class="add-cart" @click="addCar(goodDetail)">加入购物车</button>
        <button class="buy-now" @click="addCar(goodDetail)">立即购买</button>
      </div>
    </div>
    <!-- <detailSale v-show="show" class="animated slideInUp" ref="detailSale"></detailSale>   -->
    <!-- <detailServer v-show="show" class="animated slideInUp" ref="detailServer"></detailServer>   -->
   </div>
</template>
<script>
import detailSale from './detailSale'
import detailServer from './detailServer'

export default {
  data () {
    return {
      show: false,
      goods:[],
      goodDetail:{},
      detailData: [],
      gid: '',
      addSuccess: false,
      lunbo: [],
      arr:[]
    }
  },
  methods: {
    buyNow () {
      this.addCart()
    },
    showSale () {
      this.$refs.detailSale.showSale()
    },
    showServer () {
      this.$refs.detailServer.showServer()
    },
    showGoods () {
      this.$refs.detailGoods.showGoods()
    },
    store () {
      var that = this
      that.axios.post('http://www.ethedot.com/chatshop/Index/store', {
        id: sessionStorage.getItem('id')
      })
      .then(function (response) {
        if (response.data.length > 0) {
          var arr = []
          for (var i = 0; i < response.data.length; i++) {
            arr.push(response.data[i].gid)
          }
          if (arr.indexOf(that.gid) < 0) {
            that.axios.post('http://www.ethedot.com/chatshop/Index/addStore', {
              gid: that.gid
            })
            .then(function (response) {
              if (response.data === 1) {
                alert('收藏成功！')
              } else if (response.data === 2) {
                alert('收藏失败！')
              }
            })
            .catch(function (error) {
              console.log(error)
            })
          } else {
            alert('收藏成功！')
          }
        } else {
          that.axios.post('http://www.ethedot.com/chatshop/Index/addStore', {
            gid: that.gid
          })
          .then(function (response) {
            if (response.data === 1) {
              alert('收藏成功')
            } else if (response.data === 2) {
              alert('收藏失败！')
            }
          })
          .catch(function (error) {
            console.log(error)
          })
        }
      })
      .catch(function (error) {
        console.log(error)
      })
    },
    addCar(item){
      // new Promise(() =>{
      //   let arr = sessionStorage.getItem('list',JSON.parse(item))?sessionStorage.getItem('list',JSON.parse(item)):[];
      // }).then(result){

      // }
      console.log(sessionStorage.getItem('list'))
      if( sessionStorage.getItem('list') == '' || sessionStorage.getItem('list') == null){
        this.$data.arr.push(item)
        sessionStorage.setItem('list',JSON.stringify(this.$data.arr))
      }else{
        this.$data.arr = JSON.parse(sessionStorage.getItem('list'))
        this.$data.arr.push(item)
        sessionStorage.setItem('list',JSON.stringify(this.$data.arr))
      }
    }
  },
  components: {
    detailSale,
    detailServer
  },
  created(){
    var pid = this.$route.query.gid
    // this.distinguish()
    var that = this
    this.axios.get('http://47.97.80.89:8082/CGXT/product/showProduct',{
      params:{
        id: pid
      }
    })
    .then(function (response) {
      //处理返回数据
          that.goodDetail = response.data.pro  
    })
    .catch(function (error) {
      console.log(error)
    })
  }
}
</script>

<style lang='less'>
@import '../../common/animate.css';
p{
  margin: 0;
  padding: 0;
}
li{
  list-style: none;
}
.detail{
  position: fixed;
  left: 0;
  top: 0px;
  bottom: 50px;
  width: 100%;
  z-index: 2;
  background: #fff;
  .detailContent{
    position: absolute;
    width: 100%;
    top: 0;
    bottom: 50px;
    overflow-y: scroll;
    &::-webkit-scrollbar{
      display: none;
    }
  }
  .banner{
    height: 370px;
    overflow: hidden;
    position: relative;
    .mint-swipe{
      height: 100%;
        .mint-swipe-indicator{
          opacity: 1;
          background: #fff;
        }
        .mint-swipe-indicator.is-active{
          background: #f29600;
        }
    }
    span{
      position: absolute;
      left: 5%;
      top: 5%;
      color: #ebebeb;
      font-size: 20px;
      font-weight: bold;
      z-index: 10;
    }
    img{
      width: 100%;
      height: 100%;
      display: inline-block;
    }
    .mint-cell{
      position: absolute;
      width: 100%;
      bottom: 0;
      background: #000;
      .mint-cell-text{
        color:#f29600;
        font-weight: 500;
        font-size: 16px;
        display: inline-block;
        line-height: 48px;
      }
    }
  }
  .introduce{
    height: 50px;
    background: #f8f8f8;
    p{
      padding: 0 2%;
      line-height: 50px;
    }
  }
  .price{
    .now-price,.old-price,.delivery{
      width: 90%;
      margin: 0 auto;
    }
    .now-price{
      line-height: 35px;
      font-size: 16px;
      color: red;
    }
    .old-price{
      font-size: 14px;
      color: #666;
      line-height: 25px;
    }
    .delivery{
      height: 40px;
      .delivery-left{
        float: left;
        line-height: 40px;
        color: #666;
      }
      .delivery-right{
        float: right;
        line-height: 40px;
        color: #666;
      }
    }
  }
  .sale,.promise,.product-info{
    height: 40px;
    background: #fff;
    padding: 0 5%;
    color: #333;
    .arrow-right{
      float: right;
      line-height: 40px;
    }
  }
  .sale{
    .sale-left{
      float: left;
      line-height: 40px;
    }
    border-bottom: 1px solid #ddd;
  }
  .promise{
    border-bottom: 1px solid #ddd;
    .promise-container{
      p{
        float: left;
        width: 50%;
        line-height: 40px;
        span{
          color: #f29600;
          margin-right: 2%;
        }
      }
    }
  }
  .product-info{
    .product-left{
      float: left;
      line-height: 40px;
    }
  }
  .detail-footer{
    z-index: 3;
    width: 100%;
    height: 50px;
    position: fixed;
    bottom: 49px;
    border-top: 1px solid #ebebeb;
    background: #fff;
    .left{
      width: 40%;
      float: left;
      height: 50px;
      text-align: center;
      font-size: 0;
      margin: 0;
      border: none;
      p{
        font-size: 14px;
        line-height: 20px;
        color:#666;
      }
      img{
        width: 20px;
        height: 20px;
        display: inline-block;
        margin-top: 8px;
      }
      .store{
        width: 100%;
        float: right;
        height: 50px;
      }
    }
    .right{
      float: right;
      width: 60%;
      height: 50px;
      font-size: 0;
      button{
        padding: 0;
        margin: 0;
        border: none;
        height: 50px;
        font-size: 14px;
        color:#ebebeb;
      }
      .add-cart{
        background: #999;
        width: 45%;
        float: left;
        outline: none;
      }
      .buy-now{
        background: #f29600;
        width: 55%;
        float: right;
      }
    }
  }
}
.product_ul{
  li{
    list-style: none;
    background: #fff;
    width: 30%!important;
    float: left;
    margin-left: 3%;
    margin-top: 10px;
    img{
      // height: 120px;
      width: 100%;
      display: block;
    }
    p{
      font-size: 14px;
      text-align: center;
      color: #333;
      line-height: 22px;
      text-overflow:ellipsis;
      padding: 0 2px;
    }
    .yuan{
      float: left;
      color: #fa0000;
      line-height: 40px;
      padding-left: 10px;
    }
    .car{
      background: url('./car.png') no-repeat;
      background-size: 100% 100%;
      width: 20px;
      height: 24px;
      float: right;
      margin: 8px 10px 0 0;
    }
  }
}
</style>
