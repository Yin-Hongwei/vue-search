<template>
  <div>
    <logo-select @getindex='getIndex'></logo-select>
    <div class="search-input">
      <input type="text" v-model="keyword" @keyup="get($event)" @keydown.enter="search()" @keydown.down="selectDown()" @keydown.up.prevent="selectUp()">
      <!--删除键X-->
      <span class="search-reset" @click="clearInput()">&times;</span>
      <button class="search-btn" @click="search()">搜索</button>
      <div class="search-select">
        <transition-group name="itemfade" tag="ul" mode="out-in" v-cloak>
          <li v-for="(value,index) in myData" :class="{selectback:index==now}" class="search-select-option search-select-list" @mouseover="selectHover(index)" @click="selectClick(index)" :key="value">
            {{value}}
          </li>
        </transition-group>
      </div>
    </div>
  </div>
</template>

<script>
import logoSelect from './Logo'

export default {
  components: {
    'logo-select': logoSelect
  },
  data: function () {
    return {
      myData: [],
      keyword: '',
      now: -1,
      searchIndex: 0,
      logoData: [{
        name: 'baidu',
        searchSrc: 'https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=0&rsv_idx=1&tn=baidu&wd='
      },
      {
        name: 'google',
        searchSrc: 'https://google.com'
      }
      ]
    }
  },
  methods: {
    get: function (e) {
      if (e.keyCode === 38 || e.keyCode === 40) {
        return
      }
      this.$http.jsonp('http://sug.so.360.cn/suggest?word=' + this.keyword + '&encodein=utf-8&encodeout=utf-8').then(function (res) {
        this.myData = res.data.s
      })
    },
    selectDown: function () {
      this.now++
      if (this.now === this.myData.length) {
        this.now = 0
      }
      this.keyword = this.myData[this.now]
    },
    selectUp: function () {
      this.now--
      if (this.now === -1) {
        this.now = this.myData.length - 1
      }
      this.keyword = this.myData[this.now]
    },
    search: function () {
      window.open(this.logoData[this.searchIndex].searchSrc + this.keyword)
    },
    selectHover: function (index) {
      this.now = index
    },
    selectClick: function (index) {
      this.keyword = this.myData[index]
      this.search()
    },
    clearInput: function (index) {
      this.keyword = ''
      this.$http.jsonp('https://sug.so.360.cn/suggest?word=' + this.keyword + '&encodein=utf-8&encodeout=utf-8').then(function (res) {
        this.myData = res.data.s
      })
    },
    getIndex: function (index) {
      this.searchIndex = index
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style type="text/css">
.search-input {
  height: 45px;
  width: 600px;
  margin: 0 auto;
  margin-top:10px;
  position:relative;
}
.search-input input {
  border: 1px solid #e4e4e4;
  box-sizing: border-box;
  width: 500px;
  height: 45px;
  float: left;
  padding-left: 10px;
  padding-right: 10px;
  font-size: 18px;
  overflow: hidden;
}
.search-btn {
  height: 45px;
  width: 100px;
  border: 1px solid dodgerblue;
  background-color: dodgerblue;
  color: white;
  font-size: 16px;
  font-weight: bold;
  float: left;
  cursor: pointer;
}
.search-reset {
  width: 21px;
  height: 21px;
  position: absolute;
  display: block;
  line-height:21px;
  text-align: center;
  cursor: pointer;
  font-size: 20px;
  right: 110px;
  top: 12px;
}
.search-select {
  position: absolute;
  top: 45px;
  width: 500px;
  box-sizing: border-box;
  z-index: 999;
}
.search-select li {
  border: 1px solid #d4d4d4;
  border-top: none;
  border-bottom: none;
  background-color: #fff;
  width: 100%
}

.search-select-option {
  box-sizing: border-box;
  padding: 7px 10px;
}
.search-select-list {
  transition: all 0.5s
}
.selectback {
  background-color: #eee !important;
  cursor: pointer;
}
.input::-ms-clear {
  display: none;
}
.itemfade-enter,.itemfade-leave-active {
  opacity: 0;
}
.itemfade-leave-active {
  position: absolute;
}
.selectback {
  background-color: #eee !important;
  cursor: pointer
}
.search-select ul{
  margin:0;
  text-align: left;
}
</style>
