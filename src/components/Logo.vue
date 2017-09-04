<template>
  <div class="logo-wrap">

    <img :src="logoData[selectedNow].imgSrc" @click="toggleLogo">
    <span class="triangel-down" @click="toggleLogo"></span>

    <transition name="logofade">
      <ul v-show="showLogoList" class="logoList">
        <li
          v-for="(item,index) in logoData"
          class="logoItem"
          @mouseover="itemHighLight(index)"
          :class="{hightlight:index==selectItem}"
          @click="chooseLogo(index)">
          <img :src="item.imgSrc">
        </li>
      </ul>
    </transition>

  </div>
</template>

<script>
  export default {
    data() {
      return {
        selectedNow: 0,
        selectItem: -1,
        showLogoList: false,
        logoData: [{
          imgSrc: require('../assets/360_logo.png')
        }, {
          imgSrc: require('../assets/baidu_logo.png')
        }, {
          imgSrc: require('../assets/sougou_logo.png')
        }]
      }
    },
    methods: {
      toggleLogo() {
        this.showLogoList = !this.showLogoList
      },
      itemHighLight(index) {
        this.selectItem = index;
      },
      chooseLogo: function (index) {
        this.selectedNow = index;
        this.showLogoList = false;
        this.$emit('getCategory', this.selectedNow);
      }
    }
  }
</script>

<style scoped type="text/css">
  .logo-wrap {
    width: 600px;
    height: 140px;
    position: relative;
    margin: 0 auto;
  }

  .logo-wrap > img {
    margin-left: 20px;
    cursor: pointer;
  }

  .triangel-down {
    position: absolute;
    width: 0;
    height: 0;
    border-left: 10px solid transparent;
    border-right: 10px solid transparent;
    border-top: 14px solid rgba(0, 0, 0, .5);
    right: 100px;
    top: 66px;
    cursor: pointer
  }

  .logoList {
    position: absolute;
    top: 100%;
    width: 380px;
    left: 30%;
    margin-left: -100px;
    z-index: 999999;
    border: 1px solid #d4d4d4
  }

  .logoItem {
    list-style: none;
    width: 100%;
    height: 100px;
    background: #eee;
  }

  .logoItem img {
    width: 100%;
    cursor: pointer;
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

  .hightlight {
    background-color: #ddd;
  }

  @media screen and (max-width: 768px){
    .logo-wrap{
      width:100%;
      position: relative;
    }
    .logo-wrap > img{
      width: 100%;
      position: absolute;
      left: -25px;
    }
    .triangel-down{
      display: none;
    }
    .logoList{
      width: 80%;
      margin: 0 auto;
      position: absolute;
      top: 100px;
      left: 80px;
      border: none;
    }
    .logoItem{
      height: 70px;
      width: 80%;
    }
    .logoItem img{
      height: 70px;
      width: 100%;
      text-align: center;
    }
  }
</style>
