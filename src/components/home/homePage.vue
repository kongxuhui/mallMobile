<template>
  <div class="homePage">
    <div class="search-top">
      <div class="search">
          <router-link to="/home/search">
            <i class="el-icon-search"></i>
            <input type="text">
            <button class="btn-search">搜索</button>
          </router-link>
      </div>
    </div>
    <div class="category">
      <el-row :gutter="10">

        <el-col :span="8" v-for="(item,index) in list" :key='index'><div class="grid-content bg-purple">
          <router-link :to="{ path:'/catalog', query:{id:item.id}}"><img :src="item.imgurl" alt=""></router-link>
        </div></el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      goods: [],
      list:[
        
        {imgurl:'http://ssdd.xiaovip.com.cn/attachment/images/2/2019/06/cPKwiDLNQaqfP8elThED88e4nPweeE.jpg',id:'1'},
        {imgurl:'http://ssdd.xiaovip.com.cn/attachment/images/2/2019/06/lPkf11kyfYrK78hR9rNr1p1mzpSaP9.jpg',id:'2'},
        {imgurl:'http://ssdd.xiaovip.com.cn/attachment/images/2/2019/06/Xv1LL1wHOfllL8wLPffOf1fHs1c48f.jpg',id:'3'},
        {imgurl:'http://ssdd.xiaovip.com.cn/attachment/images/2/2019/06/vk77mfTvhNOu9fnnZYo4hHtFeOkFvF.jpg',id:'4'},
        {imgurl:'http://ssdd.xiaovip.com.cn/attachment/images/2/2019/06/nZdbP36BbkOBNwTBbc4SjsN9Bt54lc.jpg',id:'5'},
        {imgurl:'http://ssdd.xiaovip.com.cn/attachment/images/2/2019/06/I2g52405w44b0AWsyBgxx4AG1Hk1a2.jpg',id:'6'},
        {imgurl:'http://ssdd.xiaovip.com.cn/attachment/images/2/2019/06/JWoNvLv7H7i2Wo127Zb7y77coNCvNl.jpg',id:'7'},
        {imgurl:'http://ssdd.xiaovip.com.cn/attachment/images/2/2019/06/L3Euj8EKo8O9ew9uDOd8WUpEeKZk2Q.jpg',id:'8'},
        {imgurl:'http://ssdd.xiaovip.com.cn/attachment/images/2/2019/06/B6zPypMgYPzpVIwW2U0zi0W6HV9wh9.jpg',id:'9'}
      ],
      banners: [],
      loading: false
    }
  },
  methods: {
  },
  mounted: function () {
    this.distinguish()
    var that = this
    this.loading = true
    this.axios.post('http://www.ethedot.com/chatshop/Index/test')
    .then(function (response) {
      that.loading = false
      for (var i = 0; i < response.data.length; i++) {
        that.goods.push({click: response.data[i].click, gid: response.data[i].gid, name: response.data[i].name, price: response.data[i].price, src: 'http://www.ethedot.com/chatshop/Public/Uploads/' + response.data[i].pic})
      }
    })
    .catch(function (error) {
      console.log(error)
    })
  }
}
</script>

<style lang='less'>
body{
  margin: 0;
} 
li{
 list-style: none;
}
p{
  padding: 0;
  margin: 0;
}
.homePage {
  margin-bottom: 60px;
  position: relative;
  overflow-y: scroll;
  .category{
    padding: 10px 2%;
    .bg-purple {
        height: 170px;
        // background: #ccc;
        img{
          display: block;
          width: 100%;
          height: 100%;
          object-fit:cover;
        }
    }
  }
  &::-webkit-scrollbar{
    display: none;
  }
  .search-top {
    background: #eee;
    height: 50px;
    .el-icon-arrow-left {
      line-height: 50px;
      margin-left: 2%;
    }
    .search {
      height: 30px;
      width: 80%;
      vertical-align: top;
      margin-top: 10px;
      margin-left: 10%;
      border-radius: 8px;
      display: inline-block;
      overflow: hidden;
      font-size: 0;
      background: #fff;
      .el-icon-search {
        margin: 0 2%;
        height: 30px;
        line-height: 30px;
        width: 6%;
        color: #fa0000;
        font-size: 15px;
      }
      input {
        height: 30px;
        vertical-align: top;
        outline: none;
        border: none;
        width: 75%;
      }
      .btn-search {
        border: none;
        background: #fa0000;
        color: #fff;
        height: 30px;
        line-height: 30px;
        width: 15%;
        vertical-align: top;
        margin: 0;
        padding: 0;
      }
    }
  }
  .top{
        position:absolute;
        height: 30px;
        top: 50px;
        z-index: 1;
        width: 80%;
        left: 10%;
         p{
            margin: 0;
            padding: 0;
            line-height: 30px;
            color: #bbb;
        }
        .location,.name,.search{
            float: left;
            width: 33.3%;
            height: 30px;
        }
        .location{
            text-align: left;
            img{
                width: 12px;
                margin-right: 4%;
                float: left;
                margin-top: 6px;
            }
            .city{
                margin: 0;
                padding: 0;
                float:left;
                line-height: 30px;
            }
            .el-icon-caret-bottom{
                float: left;
                margin-left: 4%;
                font-size: 12px;
                margin-top: 10px;
                color: #bbb;
            }
        }
    }  
}
</style>
