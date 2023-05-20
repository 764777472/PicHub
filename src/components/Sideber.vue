<script setup lang="ts">
import ImageModal from './ImageModal.vue'
import FolderModal from './FolderModal.vue'
import LewButton from './base/LewButton.vue'
import { Alert } from '../util/alert'
import {
  LewFormItem,
  LewSwitch
} from '../components/base'

import axios from '../axios/http'
import { onMounted, ref, watch } from 'vue'

import { useRoute, useRouter } from 'vue-router'
import { GithubConfig } from '../model/github_config.model'
const route = useRoute()
const router = useRouter()
const emit = defineEmits(['SetImageModal','hideBar','setFloder'])

let github_config: GithubConfig = JSON.parse(
  localStorage.getItem('github_config') as any
)

let folders = ref([] as any)
let isOpenImageModal = ref(false)
let isOpenFolderModal = ref(false)
let loading = ref(false)


const props = defineProps({ showSideber: Boolean as any, isMobile: Boolean as any})

onMounted(() => {
  if (github_config?.repoId) {
    GetFolders()
  }
})

watch(
  () => route.query,
  (n: any) => {
    isOpenFolderModal.value = false
    if (
      !github_config?.owner &&
      route.path != '/setting' &&
      route.path != '/about'
    ) {
      router.push('/setting')
    } else if (route.query.reload) {
      GetFolders()
    }
  }
)

watch(
  () => isOpenImageModal.value,
  (n: any) => {
    if (isOpenImageModal.value) {
      emit('SetImageModal', true)
    } else {
      emit('SetImageModal', false)
    }
  }
)

const GetFolders = () => {
  loading.value = true
  axios
    .get({
      url: `/repositories/${
        github_config.repoId
      }/contents?t=${new Date().getTime()}`,
    })
    .then((res: any) => {
      loading.value = false
      folders.value = res.data.filter((e) => e.type == 'dir')
      if (route.path == '/' && !route.query.folder) {
        router.push(`/?folder=${folders.value[0].name}`)
      }
    })
    .catch(() => {
      loading.value = false
    })
}

let isDark = ref(localStorage.getItem('isDark') ? true : false)
const changeDarkModel = (e) => {
  if (e.target.checked) {
    document.getElementsByTagName('html')[0].classList.add('dark')
    localStorage.setItem('isDark','true')
  } else {
    document.getElementsByTagName('html')[0].classList.remove('dark')
    localStorage.removeItem('isDark');
  }
}
const changeImageModel = () => {
  if (!github_config?.repoId) {
    Alert({
      type: 'danger',
      text: '请前往设置，完成配置信息',
    })
    return
  }
  isOpenImageModal.value = !isOpenImageModal.value
}

defineExpose({
  changeImageModel,
})
</script>

<template>
  <div class="wrapper">
    <!-- 遮罩 -->
    <div @click="emit('hideBar', false)" v-if="isMobile && showSideber" class="cover-fix"></div>
    <!-- 侧边栏  -->
    <div v-if="showSideber" class="sidebar">
      <div class="header">
        <div class="mask"></div>
        <div class="logo">Pic<span>Hub</span></div>
      </div>
      <!-- 文件夹列表 -->
      <div class="folder" :class="{ loading: loading }">
        <a
          v-for="(item, index) in folders"
          :key="index"
          class="item"
          @click="emit('setFloder', item.name)"
          :class="{ active: route.query.folder == item.name }"
          :href="`/#/?folder=${item.name}`"
        >
          {{ item.name }}
          <span class="status-point"></span>
        </a>

        <div v-show="!loading" class="tips">
          <span v-if="folders.length != 0">{{ folders.length }} folders</span>
          <span v-if="github_config?.owner && folders.length == 0"
            >暂无文件夹
          </span>
          <span v-if="!github_config?.owner">未授权</span>
        </div>
        <lew-button
          v-show="!loading"
          v-if="github_config?.owner && folders.length == 0"
          style="margin-top: 10px"
          type="primary"
          @click="isOpenFolderModal = !isOpenFolderModal"
        >
          立即创建
        </lew-button>
      </div>

      <!-- 操作栏 -->
      <folder-modal
        @close="isOpenFolderModal = false"
        @updateFolderList="GetFolders()"
        :isOpen="isOpenFolderModal"
      ></folder-modal>
      <div class="handle-box">
        <!-- 创建文件夹 -->
        <lew-button @click="changeImageModel"> 图片上传 </lew-button>

        <lew-button @click="isOpenFolderModal = !isOpenFolderModal">
          创建文件夹
        </lew-button>
        <a @click="emit('hideBar', false)" href="/#/setting" style="margin-bottom: 7px;"> <lew-button> 设置 </lew-button></a>
        <lew-form-item direction="row" center title="暗黑模式" style="height: 45px;">
          <lew-switch v-model="isDark" @change="changeDarkModel"></lew-switch>
        </lew-form-item>
        <!-- <a href="/#/about"> <lew-button> 关于 </lew-button></a> -->
      </div>
    </div>
    <!-- 上传图片 -->
    <image-modal
      :isOpen="isOpenImageModal"
      :folders="folders"
      :isMobile="isMobile"
      @Close="changeImageModel"
    ></image-modal>
  </div>
</template>
<style>
.cover-fix {position: fixed;top: 0;bottom: 0;left: 0;right: 0;background: rgba(0,0,0,.25);z-index: 90;}
.image-info .item {
  font-size: 14px;
  margin-bottom: 20px;
}
.image-info div {
  word-wrap: break-word;
  width: 100%;
}
.image-info div span {
  height: 20px;
  line-height: 20px;
  font-size: 14px;
  color: #999;
}
</style>

<style lang="scss" scoped>
.sidebar {
  position: fixed;
  left: 0;
  top: 0;
  width: 200px;
  border-right: var(--border-width) var(--border-color) solid;
  box-sizing: border-box;
  height: 100vh;
  overflow-y: scroll;
  overflow-x: hidden;
  z-index: 99;
  background-color: var(--background);
  animation: toRight 0.1s linear;
  @keyframes toRight {
    from {left: -200px}
    to {left: 0;}
  }
  .header {
    position: fixed;
    height: 70px;
    width: 200px;
    display: flex;
    z-index: 99;
    align-items: center;
    padding: 20px;
    box-sizing: border-box;
    border-right: var(--border-width) var(--border-color) solid;
    border-bottom: var(--border-width) var(--border-color) solid;

    .mask {
      position: absolute;
      left: 0px;
      top: 0px;
      width: 100%;
      height: 100%;
      background-color: var(--background);
      z-index: -1;
    }
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
  .folder {
    position: relative;
    margin-top: 70px;
    padding-bottom: calc(45px * 4 + 38px);
    background: var(--background);
    min-height: calc(100vh - 400px);
    padding: 14px 7px;
    box-sizing: border-box;
    .tips {
      display: flex;
      justify-content: center;
      align-items: center;
      text-align: center;
      flex-direction: column;
      padding: 20px;
      color: var(--text-color-2);
      opacity: 0.7;
    }
    .item {
      position: relative;
      display: flex;
      align-items: center;
      height: 45px;
      line-height: 50px;
      box-sizing: border-box;
      padding: 0px 20px;
      width: 100%;
      white-space: nowrap;
      border-radius: 12px;
      text-overflow: ellipsis;
      overflow: hidden;
      color: var(--text-color-2);
      cursor: pointer;
      .status-point {
        width: 10px;
        height: 10px;
        margin-left: 12px;
        border-radius: 10px;
        display: inline-block;
        background: #27c24c;
        transition: opacity 0.25s;
        opacity: 0;
      }
    }
    .active {
      .status-point {
        opacity: 1;
      }
    }
    .item:hover {
      color: var(--text-color);
      background: var(--hover-bg-color);
    }
    .item:last-child {
      justify-content: center;
      font-size: 14px;
    }
    .item:last-child:hover {
      color: var(--text-color-2);
      background: none;
    }
  }
  .folder::after {
    position: absolute;
    top: 10%;
    left: 50%;
    content: '';
    border: 4px solid rgba(194, 194, 194, 0.4);
    border-left-color: var(--primary-color);
    border-radius: 50%;
    width: 14px;
    height: 14px;
    opacity: 0;
    animation: donut-spin 0.8s linear infinite;
    transition: all 0.15s;
    transform: translate(-50%, -50%);
  }

  .loading::after {
    opacity: 1;
  }
  @keyframes donut-spin {
    0% {
      transform: translate(-50%, -50%) rotate(0deg);
    }
    100% {
      transform: translate(-50%, -50%) rotate(360deg);
    }
  }
  .handle-box {
    width: 200px;
    height: calc(45px * 4 + 38px);
    position: fixed;
    bottom: 0px;
    background: var(--background-2);
    border-top: var(--border-width) var(--border-color) solid;
    border-right: var(--border-width) var(--border-color) solid;
    padding: 7px;
    box-sizing: border-box;
    z-index: 99;
  }
}
</style>
