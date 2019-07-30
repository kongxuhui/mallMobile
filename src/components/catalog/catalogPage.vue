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
export default {
  data () {
    return {
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
      recommend: [
        {id:'0',catalog: '全部'},
        {id:'7',catalog: '休闲零食'},
        {id:'8',catalog: '生活用品'},
        {id:'9',catalog: '粮油调味'},
        {id:'1',catalog: '水饮冲调'},
        {id:'2',catalog: '香烟'},
        {id:'3',catalog: '方便速食'},
        {id:'4',catalog: '中外名酒'},
        {id:'5',catalog: '生活服务'},
        {id:'6',catalog: '快递驿站'}
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
      if(this.$data.arr.length == 0){
        this.$data.arr.push(item);
        sessionStorage.setItem('list',JSON.stringify(this.$data.arr));
      }else{
        this.$data.arr = JSON.parse(sessionStorage.getItem('list'));
        this.$data.arr.push(item);
        sessionStorage.setItem('list',JSON.stringify(this.$data.arr));
      }
      
      console.log(item)
    },
    changItem (item, key) {
      this.activeId = item.id
      // this.goods = []
      // var that = this
      // this.axios.post('http://www.ethedot.com/chatshop/Index/test', {
      //   id: sessionStorage.getItem('id')
      // })
      // .then(function (response) {
      //   for (var i = 0; i < response.data.length; i++) {
      //     if (response.data[i].tid.match(item.tid)) {
      //       that.goods.push({gid: response.data[i].gid, name: response.data[i].name, src: 'http://www.ethedot.com/chatshop/Public/Uploads/' + response.data[i].pic})
      //     }
      //   }
      // })
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
    }
    // if(param)
  //   this.loading = true
  //   this.distinguish()
  //   var that = this
  //   // 获取分类
  //   this.axios.post('http://www.ethedot.com/chatshop/Index/typelist', {
  //     id: sessionStorage.getItem('id')
  //   })
  //   .then(function (response) {
  //     that.loading = false
  //     for (var i = 0; i < response.data.length; i++) {
  //       that.recommend.push({tid: response.data[i].tid, catalog: response.data[i].comment})
  //     }
  //   })
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
