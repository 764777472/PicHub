<script setup lang="ts">
import Sideber from './components/Sideber.vue'
import { ref, onMounted, watch } from 'vue'

let isOpenImageModal = ref(false)
let isLoading = ref(false)

const SetImageModal = (value) => {
  isOpenImageModal.value = value
}

const SetLoading = (value) => {
  isLoading.value = value
}

const sideberRef: any = ref(null)
const OpenUploadModel = () => {
  sideberRef.value.changeImageModel()
}



import { useRoute } from 'vue-router'
const routes = useRoute()
// 监听路由变化
watch(
  () => routes.query,
  (n: any) => {
    setFloder(n.folder ? n.folder : '')
  }
)

// 初始化UA
let isMobile = window.innerWidth < 640 ? ref(true) : ref(false);
const SetMobile = (value) => {
  isMobile.value = value
}

// 侧边栏展示状态
let showSideber = window.innerWidth < 640 ? ref(false) : ref(true);
const SetSidSta = (value) => {
  // console.log('调用切换',value)
  showSideber.value = value
}

// 选中文件夹
let currentFlod = ref("");
const setFloder = (value) => {
  // console.log('选中文件夹',isMobile.value, value)
  currentFlod.value = value
  if(isMobile.value) {
    console.log('phone UA')
    SetSidSta(false)
  }
}

// 尺寸变化，设置UA
const windowResize = ()=>{
  let val = window.innerWidth < 640 ? true : false;
  SetMobile(val);
  if(!val) SetSidSta(true);
}
window.removeEventListener('resize',windowResize)
window.addEventListener('resize',windowResize)


onMounted(() => {
  if (localStorage.getItem('isDark')) {
    document.getElementsByTagName('html')[0].classList.add('dark')
  } else {
    document.getElementsByTagName('html')[0].classList.remove('dark')
  }
  // if (!!localStorage.getItem('github_config')) {
  //   let github_config = JSON.parse(localStorage.getItem('github_config') as any)
  //   if (github_config.isDark) {
  //     document.getElementsByTagName('html')[0].classList.add('dark')
  //   } else {
  //     document.getElementsByTagName('html')[0].classList.remove('dark')
  //   }
  // }
})
</script>

<template>
  <div class="wrapper" :style="{paddingTop: !isMobile ? '0':'70px'}">
    <div v-if="isMobile" class="github-corner mobileHead">
      <a href="/" v-if="!currentFlod">
        <div class="header">
          <div class="logo">Pic<span>Hub</span></div>
        </div>
      </a>
      <a v-if="currentFlod">
        <div class="header">
          <div class="logo">/<span>{{currentFlod}}</span> 10</div>
        </div>
      </a>
      <div @click="SetSidSta(true)" class="menuBox">
        <svg class="menus" t="1684554375409" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="19400" width="48" height="48">
          <path d="M172.1 163.58m113.38 0l65.44 0q113.38 0 113.38 113.38l0 65.44q0 113.38-113.38 113.38l-65.44 0q-113.38 0-113.38-113.38l0-65.44q0-113.38 113.38-113.38Z" fill="currentcolor" p-id="19401"></path>
          <path d="M350.92 481.78h-65.42a139.54 139.54 0 0 1-139.4-139.38v-65.44a139.54 139.54 0 0 1 139.4-139.38h65.42a139.54 139.54 0 0 1 139.38 139.38v65.44a139.54 139.54 0 0 1-139.38 139.38z m-65.42-292.2a87.48 87.48 0 0 0-87.4 87.38v65.44a87.48 87.48 0 0 0 87.4 87.38h65.42a87.48 87.48 0 0 0 87.38-87.38v-65.44a87.48 87.48 0 0 0-87.38-87.38z" fill="currentcolor" p-id="19402"></path>
          <path d="M546.34 160.44m113.38 0l65.44 0q113.38 0 113.38 113.38l0 65.44q0 113.38-113.38 113.38l-65.44 0q-113.38 0-113.38-113.38l0-65.44q0-113.38 113.38-113.38Z" fill="currentcolor" p-id="19403"></path>
          <path d="M725.16 478.64h-65.44a139.54 139.54 0 0 1-139.38-139.38v-65.42a139.54 139.54 0 0 1 139.38-139.4h65.44a139.54 139.54 0 0 1 139.38 139.4v65.42a139.54 139.54 0 0 1-139.38 139.38z m-65.44-292.2a87.48 87.48 0 0 0-87.38 87.4v65.42a87.48 87.48 0 0 0 87.38 87.38h65.44a87.48 87.48 0 0 0 87.38-87.38v-65.42a87.48 87.48 0 0 0-87.38-87.4z" fill="currentcolor" p-id="19404"></path>
          <path d="M172.1 531.18m113.38 0l65.44 0q113.38 0 113.38 113.38l0 65.44q0 113.38-113.38 113.38l-65.44 0q-113.38 0-113.38-113.38l0-65.44q0-113.38 113.38-113.38Z" fill="currentcolor" p-id="19405"></path>
          <path d="M350.92 849.38h-65.42A139.56 139.56 0 0 1 146.1 710v-65.44a139.54 139.54 0 0 1 139.4-139.38h65.42a139.54 139.54 0 0 1 139.38 139.38V710a139.54 139.54 0 0 1-139.38 139.38z m-65.42-292.2a87.48 87.48 0 0 0-87.4 87.38V710a87.48 87.48 0 0 0 87.4 87.38h65.42A87.48 87.48 0 0 0 438.3 710v-65.44a87.48 87.48 0 0 0-87.38-87.38z" fill="currentcolor" p-id="19406"></path>
          <path d="M546.34 528.04m113.38 0l65.44 0q113.38 0 113.38 113.38l0 65.44q0 113.38-113.38 113.38l-65.44 0q-113.38 0-113.38-113.38l0-65.44q0-113.38 113.38-113.38Z" fill="currentcolor" p-id="19407"></path>
          <path d="M725.16 846.24h-65.44a139.54 139.54 0 0 1-139.38-139.38v-65.44A139.54 139.54 0 0 1 659.72 502h65.44a139.54 139.54 0 0 1 139.38 139.38v65.44a139.54 139.54 0 0 1-139.38 139.42zM659.72 554a87.48 87.48 0 0 0-87.38 87.38v65.44a87.48 87.48 0 0 0 87.38 87.38h65.44a87.48 87.48 0 0 0 87.38-87.38v-65.4A87.48 87.48 0 0 0 725.16 554z" fill="currentcolor" p-id="19408"></path>
        </svg>
      </div>
    </div>
    <div id="alert-box"></div>
    <Sideber @setFloder="setFloder" :currentFlod="currentFlod" @hideBar="SetSidSta" @SetImageModal="SetImageModal" :showSideber="showSideber" :isMobile="isMobile" ref="sideberRef"></Sideber>
    <div
      class="main"
      :class="{ loading: isLoading }"
      id="main"
      :style="
        isOpenImageModal
          ? (!isMobile ? `width:calc(100% - 700px);margin-left:700px;` : ``)
          : `width:100%;padding-left: ${!isMobile ? 200 : 0 }px;box-sizing:border-box;` + `min-height: ${isMobile ? 'calc(100vh - 70px)' : '100vh' };`
          // : `width:calc(100% - 200px)`
      "
    >
      <router-view
        @SetLoading="SetLoading"
        @OpenUploadModel="OpenUploadModel()"
      />
    </div>
  </div>
</template>

<style lang="scss" scoped>
.wrapper {
  .mobileHead {background: var(--background);}
  .header {
    height: 70px;
    display: flex;
    z-index: 99;
    align-items: center;
    padding: 0 20px;
    box-sizing: border-box;
    border-right: var(--border-width) rgba(0,0,0,0) solid;
    border-bottom: var(--border-width) rgba(0,0,0,0) solid;
    .logo {
      position: relative;
      font-size: 26px;
      font-weight: bolder;
      color: var(--text-color);
      margin-left: 5px;
      span {
          background: var(--primary-color);
          color: #fff;
          border-radius: 4px;
          padding: 0px 4px;
          margin-left: 4px;
      }
    }
  }
  .menus {width: 40px;height: 40px;color: #555;filter: invert(0.5);padding-right: 6px;}
  .menuBox:active {filter: brightness(0.95) drop-shadow(1px 2px 4px #aaa);transition: all 0.1s ease-out;opacity: 0.95;}
  
  display: flex;
  background: var(--background);
  .main {
    position: relative;
    transition: all 0.25s;
    min-height: 100vh;
  }
  .main::after {
    position: absolute;
    top: 50%;
    left: 50%;
    content: '';
    border: 4px solid rgba(180, 180, 180, 0.4);
    border-left-color: var(--primary-color);
    border-radius: 50%;
    outline: var(--background) solid 10000px;
    background: var(--background);
    width: 16px;
    height: 16px;
    opacity: 0;
    animation: donut-spin 0.8s linear infinite;
    transition: all 0.25s;
    transform: translate(-50%, -50%);
    z-index: -1;
  }

  .loading::after {
    opacity: 1;
    z-index: 1;
  }

  @keyframes donut-spin {
    0% {
      transform: translate(-50%, -50%) rotate(0deg);
    }
    100% {
      transform: translate(-50%, -50%) rotate(360deg);
    }
  }
}
</style>
