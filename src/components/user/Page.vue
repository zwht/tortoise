/**
* Created by myao on 16/11/18.
*/

<template>
  <div class="Page">
    <a javascript="javascript:;" :disabled="disabled1" target="_blank" class="shouye" @click="first" :class="{active1: active1}">首页</a>
    <a javascript="javascript:;" :disabled="disabled1" target="_blank" class="page-prev" @click="nextPage(-1)" :class="{active1: active1}">上一页</a>
    <div id="page_div" class="pagediv" v-for="item in list">
      <a href="javascript:;" @click="getData(item)" :class="{active: item.active}" >{{item.index}}</a>
    </div>
    <a javascript=":;" :disabled="disabled2" class="page-next" @click="nextPage(1)" :class="{active2: active2}">下一页</a>
    <a javascript=":;" :disabled="disabled2" target="_blank" class="weiye" @click="last" :class="{active2: active2}">尾页</a>
    <a target="_blank" class="jump" @click="jump">跳转</a>
  </div>
</template>

<script>
  export default {
    name: 'Page',
    props: ['pageNum', 'pageSize', 'totalPages', 'menu'], // 给子组件要传的参数
    data () {
      return {
        active: true,
        list: [],
        jumpPage: '',
        active1: false,
        active2: false,
        disabled1: false,
        disabled2: false
      }
    },
    watch: {
      'pageSize' (newValue, oldValue) {
        this.setInit()
      },
      'pageNum' (newValue, oldValue) {
        this.setInit()
      },
      'totalPages' (newValue, oldValue) {
        this.setInit()
      }
    },
    mounted () {
      this.setInit()
    },
    methods: {
      setInit () {
        let that = this
        let pages = Math.ceil(this.totalPages / this.pageSize)  // 总页数
        let dataArr = []
        for (let i = 1; i <= pages; i++) {
          let activeFlag = false
          if (i === that.pageNum) {
            activeFlag = true
          }
          dataArr.push({ index: i, active: activeFlag })  // index:当前页玛和active的状态
        }
        that.$set(that, 'list', dataArr)

        for (let j = 0; j < that.list.length; j++) {   // 第一页
          if (that.list[0].active) {
            that.active1 = true
            that.active2 = false
            that.disabled1 = true
          }
          if (that.list[that.list.length - 1].active) {   // 最后一页
            that.active1 = false
            that.active2 = true
          }
          if (that.list[j].index > 1 && that.list[j].index < that.list.length) {   // 多页,并且不是第一页也不是最后一页
            that.active1 = false
            that.active2 = false
          }
        }
        if (that.list.length === 1) {    //  只有一页
          that.active1 = true
          that.active2 = true
        }
      },
      getData: function (item) {
        let that = this
        for (let i = 0; i < that.list.length; i++) {
          that.list[i].active = false
        }
        item.active = true
        this.$router.push({name: 'warmingRecord', params: {pageNum: 1, pageSize: 10}})
      },
      nextPage: function (k) {
        if (this.list.length === 1) {
          return
        }
        let target = 0
        for (let i = 0; i < this.list.length; i++) {
          if (this.list[i].active) {
            target = i + k
          }
        }
        this.getData(this.list[target])
      },
      jump: function () {
        this.$router.push({name: this.menu, params: {pageNum: this.jumpPage, pageSize: this.pageSize}})
      },
      first: function () {
        if (this.list.length === 1) {
          return
        }
        this.active1 = true
        this.active2 = false
        this.pageNum = 1
        this.$router.push({name: this.menu, params: {pageNum: this.pageNum, pageSize: this.pageSize}})
        return
      },
      last: function () {
        if (this.list.length === 1) {
          return
        }
        this.active2 = true
        this.active1 = false
        this.pageNum = this.list.length
        this.$router.push({name: this.menu, params: {pageNum: this.pageNum, pageSize: this.pageSize}})
        return
      }
    }
  }
</script>


<style lang="less" rel="stylesheet/less" type="text/css">
  @import "../../assets/css/global.less";
  @import "../../assets/css/variable.less";

  .Page {
    float: right; height:40px; line-height:40px; margin:20px 0 20px 0;margin-right:20px;
    a{display: inline-block;border-right: 1px solid #ddd; height: 38px; line-height: 38px; padding: 0 15px;
      background-color:#fff;cursor: pointer;float: left;border-top: 1px solid #ddd; border-bottom: 1px solid #ddd;}
    .shouye{border-left: 1px solid #ddd;}
    a.active1{background-color: #fefefe; color:#e9e9e9; cursor: not-allowed}
    a.active2{background-color: #fefefe; color:#e9e9e9; cursor: not-allowed}
    .pagediv{display: inline-block;padding: 0;border-right: none;border-bottom: none;float:left;
      a{height: 38px; line-height: 37px; border-top:1px solid #ddd;}
      a.active{background-color: #f3f3f9}
  }
    .pagelabel{float:left}
    .jump_k{min-width: 50px; width:50px;padding: 0 5px;border: none;background-color: #fff; height: 38px; line-height: 38px;float: left;
      border-right: 1px solid #ddd;border-top: 1px solid #ddd; border-bottom: 1px solid #ddd;}
  }
</style>
