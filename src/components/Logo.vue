<template>
  <div class="main-logo">
    <img :src="logoData[selectedNow].imgSrc" @click="toggleFlag"/>
    <span class="logoList-arrow" @click="toggleFlag"></span>
    <transition name="logofade">
      <ul v-show="logoListFlag&&mouseLeaveFlag" class="logoList" @mouseleave="mouseLeaveList">
        <li v-for="(item,index) in logoData" class="logoItem" @mouseover="logoListHover(index)" :class="{selectback:index===logoNow}" @click="logoSelected(index)"  :key="(item,index)">
          <img :src="item.imgSrc">
        </li>
      </ul>
    </transition>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      selectedNow: 0, // 第几个搜索引擎
      logoNow: -1, // 记录鼠标经过的是第几个搜索引擎
      logoListFlag: false, // 是否显示切换框
      mouseLeaveFlag: false, // 鼠标是否离开
      logoData: [{
        imgSrc: require('../static/img/google_logo.png')
      },
      {
        imgSrc: require('../static/img/baidu_logo.png')
      }]
    }
  },
  methods: {
    // 切换搜索引擎
    toggleFlag: function () {
      this.logoListFlag = !this.logoListFlag
      this.mouseLeaveFlag = true
    },
    // 鼠标经过
    logoListHover: function (index) {
      this.logoNow = index
    },
    // 鼠标离开
    mouseLeaveList: function () {
      this.mouseLeaveFlag = false
      this.logoListFlag = false
    },
    // 点击时候选择
    logoSelected: function (index) {
      this.selectedNow = index
      this.logoListFlag = false
      this.$emit('getindex', this.selectedNow)
    }
  }
}
</script>

<style type="text/css">
.main-logo {
  width: 600px;
  height: 140px;
  position: relative;
}
.main-logo img {
  display: block;
  margin: 0 auto;
  user-select: none;
  cursor: pointer;
}

.logoList-arrow {
  position: absolute;
  width: 0;
  height: 0;
  border: 8px solid;
  border-color: #000 transparent transparent transparent;
  right: 100px;
  top: 66px;
  cursor: pointer
}

.logoList {
  position: absolute;
  top: 100%;
  width: 200px;
  left: 50%;
  margin-left: -100px;
  z-index: 999999;
  border: 1px solid #d4d4d4;
}
ul {
  list-style: none;
  padding: 0;
  margin: 0;
}
.logoList li {
  width: 100%;
  height: 80px;
  background-color: #FEFEFE; /*少个封号报错*/
  line-height: 80px;
  padding-top: 1px;
}

.logoList li img {
  width: 100%;
  margin-top: 10px;
}

.logofade-enter-active,
.logofade-leave-active {
  transition: all .5s;
}
.logofade-enter,
.logofade-leave-active {
  opacity: 0;
  transform: translateY(20px);
}
.selectback{
  background-color: #eee !important;
  cursor: pointer
}
</style>
