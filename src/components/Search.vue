<template>
  <div class="search-wrap">
    <logo @getCategory="getCategory"></logo>
    <div class="search-input">
      <input type="text"
             v-model="keyword"
             @keydown.enter="search"
             @keyup="getSearchList($event)"
             @keydown.up.prevent="searchListUp"
             @keydown.down="searchListDown">
      <span class="search-reset" @click="reset">&times;</span>
      <button class="search-btn" @click="search">Search</button>

      <div class="search-list" v-show="searchList">
        <transition-group name="itemfade" tag="ul" mode="out-in" v-cloak>
          <li v-for="(item,index) in searchList"
              :key="item"
              :class="{selected:index==now}"
              @mouseover="chooseItem(index)"
              @click="search"
          >
            {{item}}
          </li>
        </transition-group>
      </div>
    </div>
  </div>
</template>

<script>
  import Logo from './Logo'
  import jsonp from 'jsonp'

  export default {
    components: {
      Logo
    },
    data() {
      return {
        searchList: '', // 接受搜索数据
        now: -1,
        keyword: '',
        searchCategory: 0,
        category: [
          {
            name: '360搜索',
            searchSrc: 'https://www.so.com/s?ie=utf-8&shb=1&src=360sou_newhome&q='
          },
          {
            name: '百度搜索',
            searchSrc: 'https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=0&rsv_idx=1&tn=baidu&wd='
          },
          {
            name: '搜狗搜索',
            searchSrc: 'https://www.sogou.com/web?query='
          }
        ]
      }
    },
    compoted: {
      clear() {
        if (this.keyword == '') {
          this.now = -1
          console.log(this.now)
        }
      }
    },
    methods: {
      getSearchList(ev) {
        // 如果按得键是上或者下，就不进行数据请求
        if (ev.keyCode == 38 || ev.keyCode == 40) {
          return
        }
        jsonp(`https://sug.so.360.cn/suggest?word='${this.keyword}&encodein=utf-8&encodeout=utf-8`, null, (err, data) => {
          this.searchList = data.s
        })
      },
      searchListUp() {
        switch (this.now) {
          case -1:
            // 当开始选择时就按方向键上则跳转到最后
            this.now = this.searchList.length - 1
            this.keyword = this.searchList[this.now]
            break
          case 0:
            // 当选到第一个时候跳转到最后
            this.now = this.searchList.length - 1
            this.keyword = this.searchList[this.now]
            break
          default:
            this.now--
            this.keyword = this.searchList[this.now]
        }
        /*if(this.now==-1){
          this.now = this.searchList.length - 1
          return
        }
        this.now --
        if(this.now == -1){
          this.now = this.searchList.length - 1
        }*/
      },
      searchListDown() {
        // 键盘按下方向键下的时候
        this.now++
        // 当选到最后一个时候返回第一个
        if (this.now == this.searchList.length) {
          this.now = 0
        }
        this.keyword = this.searchList[this.now]
      },
      search() {
        // 清空搜索列表
        this.searchList = ''
        //打开对应的搜索界面
        window.open(this.category[this.searchCategory].searchSrc + this.keyword)
      },
      reset() {
        this.keyword = ''
        this.searchList = ''
      },
      chooseItem(index){
        this.now = index
        this.keyword = this.searchList[this.now]
      },
      getCategory(index) {
        this.searchCategory = index;
      }
    },
    watch:{
      keyword(val,oldVal){
        // 当搜索列表清空时，重置搜索列表高亮显示
        if(val==''){
          this.now = -1
        }
      }
    }
  }
</script>

<style scoped>
  .search-wrap {
    margin-top: 70px;
  }

  .search-input {
    width: 600px;
    margin: 30px auto;
    position: relative;
  }

  .search-input input {
    border: 1px solid #e4e4e4;
    background: rgba(255,255,255,.8);
    box-sizing: border-box;
    width: 500px;
    height: 45px;
    font-size: 18px;
    padding-left: 10px;
    padding-right: 10px;
    overflow: hidden;
  }

  .search-reset {
    position: absolute;
    right: 120px;
    top: 12px;
    cursor: pointer
  }

  .search-btn {
    display: inline-block;
    height: 45px;
    width: 100px;
    border: 1px solid rgba(60,179,113,.8);
    background-color: rgba(60,179,113,.8);
    color: white;
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
    float: right;
  }

  .search-list {
    background: #f5f5f5;
  }

  .search-list li {
    padding: 8px;
    list-style: none;
  }

  .selected {
    background: #ddd;
  }

  @media screen and (max-width: 768px){
    .search-wrap {
      margin-top: 120px;
    }
    .search-input {
      width: 80%;
      margin: 30px auto;
      position: relative;
    }
    .search-input input {
      width: 80%;
    }
    .search-reset{
      display: none;
    }
    .search-btn{
      width: 20%;
    }
  }

</style>
