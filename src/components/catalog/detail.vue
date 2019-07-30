<template>
  <div class="detail">
    <div class="detailContent">
      <div class="banner">
        <router-link to=""><span onclick="window.history.go(-1)" class="el-icon-arrow-left"></span></router-link>
      <mt-swipe :auto="0">
        <mt-swipe-item v-for="item in goodDetail.imgurl">
            <img :src="item.img" alt="">
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
          <p class="promise2"><span class="el-icon-circle-check"></span>单位：{{goodDetail.danwei}}</p>
          <p class="promise3"><span class="el-icon-circle-check"></span>库存:  {{goodDetail.kucn}}</p>
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
        <button class="add-cart" @click="addCart">加入购物车</button>
        <button class="buy-now" @click="buyNow">立即购买</button>
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
      goods:[
        {id:'1',counter:1,title:'炫迈口香糖11',price:10,total:10,thumb:'http://ssdd.xiaovip.com.cn/attachment/images/2/2019/07/dFf8n9pqh3emm8llMeIElemmfl8Mk8.png'},
        {id:'2',counter:1,title:'炫迈口香糖22',price:10,total:10,thumb:'http://ssdd.xiaovip.com.cn/attachment/images/2/2019/07/dFf8n9pqh3emm8llMeIElemmfl8Mk8.png'},
        {id:'3',counter:1,title:'炫迈口香糖33',price:10,total:10,thumb:'http://ssdd.xiaovip.com.cn/attachment/images/2/2019/07/dFf8n9pqh3emm8llMeIElemmfl8Mk8.png'},
        {id:'4',counter:1,title:'炫迈口香糖44',price:10,total:10,thumb:'http://ssdd.xiaovip.com.cn/attachment/images/2/2019/07/dFf8n9pqh3emm8llMeIElemmfl8Mk8.png'},
        {id:'5',counter:1,title:'炫迈口香糖55',price:10,total:10,thumb:'http://ssdd.xiaovip.com.cn/attachment/images/2/2019/07/dFf8n9pqh3emm8llMeIElemmfl8Mk8.png'},
        {id:'5',counter:1,title:'炫迈口香糖66',price:10,total:10,thumb:'http://ssdd.xiaovip.com.cn/attachment/images/2/2019/07/dFf8n9pqh3emm8llMeIElemmfl8Mk8.png'},
        {id:'6',counter:1,title:'炫迈口香糖77',price:10,total:10,thumb:'http://ssdd.xiaovip.com.cn/attachment/images/2/2019/07/dFf8n9pqh3emm8llMeIElemmfl8Mk8.png'},
        {id:'8',counter:1,title:'炫迈口香糖88',price:10,total:10,thumb:'http://ssdd.xiaovip.com.cn/attachment/images/2/2019/07/dFf8n9pqh3emm8llMeIElemmfl8Mk8.png'}

      ],
      goodDetail:{
        imgurl:[
          {img: 'http://ssdd.xiaovip.com.cn/attachment/images/2/2019/05/vuDYvzFGY8VSdX2ODGODF7VRgX66Bt.jpg'},
          {img: 'http://ssdd.xiaovip.com.cn/attachment/images/2/2019/05/P6u4hU46U32iu6n26Us6UUuUUQBbqz.jpg'},
          {img: 'http://ssdd.xiaovip.com.cn/attachment/images/2/2019/05/g6ONdaNfQqatAT9vAdTluuVfvvy6fU.jpg'}
        ],
        name:'香港公仔碗仔面(76g)',
        price:5.5,
        kucn:50,
        danwei:'包'
      },
      detailData: [],
      gid: '',
      addSuccess: false,
      lunbo: []
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
    addCart () {
      var that = this
      that.axios.post('http://www.ethedot.com/chatshop/Index/car', {
        id: sessionStorage.getItem('id')
      })
      .then(function (response) {
        if (response.data.length > 0) {
          var arr = []
          for (var i = 0; i < response.data.length; i++) {
            arr.push(response.data[i].gid)
          }
          if (arr.indexOf(that.gid) < 0) {
            that.axios.post('http://www.ethedot.com/chatshop/Index/caozuo', {
              gid: that.gid,
              id: sessionStorage.getItem('id')
            })
            .then(function (response) {
              if (response.data === 1) {
                that.$router.push('/cart')
              } else if (response.data === 2) {
                alert('添加失败！')
              }
            })
            .catch(function (error) {
              console.log(error)
            })
          } else {
            that.$router.push('/cart')
          }
        } else {
          that.axios.post('http://www.ethedot.com/chatshop/Index/caozuo', {
            gid: that.gid,
            id: sessionStorage.getItem('id')
          })
          .then(function (response) {
            if (response.data === 1) {
              that.$router.push('/cart')
            } else if (response.data === 2) {
              alert('添加失败！')
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
    }
  },
  components: {
    detailSale,
    detailServer
  },
  mounted: function () {
    // this.distinguish()
    // var that = this
    // this.axios.post('http://www.ethedot.com/chatshop/Index/test')
    // .then(function (response) {
    //   for (var i = 0; i < response.data.length; i++) {
    //     if (that.$route.params.gid === response.data[i].gid) {
    //       that.detailData.push(
    //         response.data[i].name,
    //         response.data[i].price,
    //         response.data[i].oldpri,
    //         response.data[i].content,
    //       )
    //       that.gid = response.data[i].gid
    //       var arr = response.data[i].lunbo
    //       for (let i = 0; i < arr.length; i++) {
    //         that.lunbo.push({
    //           'pic': arr[i]
    //         })
    //       }
    //     }
    //   }
    // })
    // .catch(function (error) {
    //   console.log(error)
    // })
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
        width: 30%;
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
    width: 25%;
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
