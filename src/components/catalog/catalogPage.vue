<template>
  <div class="catalog">
    <div class="left">
      <ul class="leftItem">
        <li v-for="(item, key) in recommend" :class="{ itemActive: activeId == item.id }" @click="changItem(item, key+1)" v-bind:key="item.id">
          {{ item.catalog }}
        </li>
      </ul>
    </div>
    <div class="right">
        <div class="rightItem">
            <!-- <div class="loadGoods" v-show="loading">
              <div class="spinner">
                <div class="rect1"></div>
                <div class="rect2"></div>
                <div class="rect3"></div>
                <div class="rect4"></div>
                <div class="rect5"></div>
              </div>
            </div> -->
            <ul>
              <li v-for="(item, index) in goods" @click="goDetail(item)" :key="index">
                <router-link :to="{ path:'/catalog/detail', query:{gid:item.id}}"><img class="animated zoomIn" :src="item.thumb" alt=""></router-link>
                <p>{{item.title}}</p>
                <p class="yuan">￥{{item.price}}</p>
                <p class="car" @click="addCar(item)"></p>
              </li>
            </ul>
        </div>
    </div>
  </div>
</template>


<script>
import qs from 'Qs'
export default {
  data () {
    return {
      goods:[],
      recommend: [

      ],
      arr:[],
      activeId: '0',
      firstTid: '',
      loading: false
    }
  },
  methods: {
    addCar(item){
      // new Promise(() =>{
      //   let arr = sessionStorage.getItem('list',JSON.parse(item))?sessionStorage.getItem('list',JSON.parse(item)):[];
      // }).then(result){

      // }
      if(sessionStorage.getItem('list') == '' || sessionStorage.getItem('list') == null){
        this.$data.arr.push(item);
        sessionStorage.setItem('list',JSON.stringify(this.$data.arr));
      }else{
        this.$data.arr = JSON.parse(sessionStorage.getItem('list'));
        this.$data.arr.push(item);
        sessionStorage.setItem('list',JSON.stringify(this.$data.arr));
      }
    },
    changItem (item, key) {
      this.activeId = item.id
      this.goods = []
      var that = this
      let data = {"type":this.activeId}
       this.axios.post('http://47.97.80.89:8082/CGXT/product/productList3', 
        qs.stringify(data)).then(function (response) {
         for (var i = 0; i < response.data.pro.list.length; i++) {
           var obj = response.data.pro.list[i];
           var imgURl = obj.proList[0].url;
           if(imgURl===null){

           }
           that.goods.push({id:obj.id,gid:obj.type,price:obj.price, title:obj.name, thumb: 'http://47.97.80.89:8082' + imgURl})
         }
       })
      // .catch(function (error) {
      //   console.log(error)
      // })
    }
    // goDetail (item) {
    //   sessionStorage.gid = item.gid
    //   // 点击率
    //   this.axios.post('http://www.ethedot.com/chatshop/Index/clickRate', {
    //     id: sessionStorage.getItem('id'),
    //     gid: item.gid
    //   })
    //   .then(function (response) {
    //   })
    //   .catch(function (error) {
    //     console.log(error)
    //   })
    //   this.$router.push('/catalog/detail/' + item.gid)
    // }
  },
  mounted: function () {
    if(this.$route.query.id){
      this.activeId = this.$route.query.id;
      this.axios.post('http://47.97.80.89:8082/CGXT/product/productList3',qs.stringify({"type":this.activeId})).then(function (response) {
         for (var i = 0; i < response.data.pro.list.length; i++) {
           var obj = response.data.pro.list[i];
           var imgURl = obj.proList[0].url;
           if(imgURl===null){

           }
           that.goods.push({id:obj.id,gid:obj.type,price:obj.price, title:obj.name, thumb: 'http://47.97.80.89:8082' + imgURl})
         }
       })
    }else{
      this.axios.post('http://47.97.80.89:8082/CGXT/product/productList3',qs.stringify({"type":"0"})).then(function (response) {
         for (var i = 0; i < response.data.pro.list.length; i++) {
           var obj = response.data.pro.list[i];
           var imgURl = obj.proList[0].url;
           if(imgURl===null){

           }
           that.goods.push({id:obj.id,gid:obj.type,price:obj.price, title:obj.name, thumb: 'http://47.97.80.89:8082' + imgURl})
         }
       })
    }
    // if(param)
  //   this.loading = true
  //   this.distinguish()
     var that = this
     let data = {"pageNum":"1","pageSize":"20"}
  //   // 获取分类
     this.axios.get('http://47.97.80.89:8082/CGXT/category/categoryList3', {
       params:{
          pageNum:"1",
          pageSize:"20"
        }
     }).then(function (response) {
       that.loading = false
       for (var   i = 0; i < response.data.pro.length; i++) {
         var obj = response.data.pro[i];
         that.recommend.push({id:obj.cid, catalog: obj.name})
       }
       that.recommend = that.recommend.reverse()
     })
       
  //   .catch(function (error) {
  //     console.log(error)
  //   })
  //   // if (sessionStorage.id === ',,') {
  //   //   alert('身份过期，请重新登录！')
  //   // }
  //   // 获取商品
  //   this.axios.post('http://www.ethedot.com/chatshop/Index/test', {
  //     id: sessionStorage.getItem('id')
  //   })
  //   .then(function (response) {
  //     // var matchId = that.recommend[0].tid
  //     for (var i = 0; i < response.data.length; i++) {
  //       if (response.data[i].tid.match(that.firstTid)) {
  //         that.goods.push({gid: response.data[i].gid, name: response.data[i].name, src: 'http://www.ethedot.com/chatshop/Public/Uploads/' + response.data[i].pic})
  //       }
  //     }
  //   })
  //   .catch(function (error) {
  //     console.log(error)
  //   })
  }
}
</script>
<style lang='less'>
@import '../../common/animate.css';
*{
  margin:0;
  padding: 0;
}
.itemActive{
  border-left: 3px solid #fa0000;
  background: #f4f4f4;
  color: #fa0000;
}
.product_ul{
  li{
    list-style: none;
    background: #fff;
    width: 45%;
    float: left;
    margin-left: 3%;
    margin-top: 10px;
    img{
      height: 120px;
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
.catalog{
  .loadGoods{
    color: red;
    z-index: 2;
    position: fixed;
  }
  .left{
    .leftItem{
        position: fixed;
        top: 0;
        bottom: 50px;
        left: 0;
        width: 25%;
        overflow-y: scroll;
        background: #fff;
      &::-webkit-scrollbar {
        display: none;
      }
      li{
        list-style: none;
        font-size: 0.8rem;
        text-align: center;
        height: 50px;
        line-height: 50px;
        border-bottom: 1px solid #ebebeb;
      }
    }
  }
  .right{
    .rightItem{
        background: #eee;
        position: fixed;
        top: 0;
        bottom: 50px;
        right: 0;
        width: 75%;
        overflow-y: scroll;
        border-right: 1px solid #ddd;
        .loadGoods{
          width: 100%;
          height: 100%;
          .spinner {
            position: absolute;
            top: 40%;
            left: 20%;
            width: 50px;
            height: 60px;
            text-align: center;
            font-size: 10px;
          }
          .spinner > div {
            background-color: #fa0000;
            height: 100%;
            width: 6px;
            display: inline-block;

            -webkit-animation: stretchdelay 1.2s infinite ease-in-out;
            animation: stretchdelay 1.2s infinite ease-in-out;
          }
          .spinner .rect2 {
            -webkit-animation-delay: -1.1s;
            animation-delay: -1.1s;
          }
          .spinner .rect3 {
            -webkit-animation-delay: -1.0s;
            animation-delay: -1.0s;
          }
          .spinner .rect4 {
            -webkit-animation-delay: -0.9s;
            animation-delay: -0.9s;
          }
          .spinner .rect5 {
            -webkit-animation-delay: -0.8s;
            animation-delay: -0.8s;
          }
          @-webkit-keyframes stretchdelay {
            0%, 40%, 100% { -webkit-transform: scaleY(0.4) }
            20% { -webkit-transform: scaleY(1.0) }
          }
          @keyframes stretchdelay {
            0%, 40%, 100% {
              transform: scaleY(0.4);
              -webkit-transform: scaleY(0.4);
            }  20% {
              transform: scaleY(1.0);
              -webkit-transform: scaleY(1.0);
            }
          }
        }
        ul{
          height: auto;
          .product_ul;
          clear: both;
        }
        &::-webkit-scrollbar {
          display: none;
        }
      .title{
        clear: both;
        color: #fa0000;
      }
    }
  }
}
</style>
