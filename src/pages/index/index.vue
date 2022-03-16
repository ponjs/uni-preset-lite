<script setup>
import { onMounted, ref } from 'vue'
import ajax from '@/common/ajax'

const guideRef = ref(null)

const baseURL = ref('')
const request = ref(null)
const result = ref('')

// 获取接口根地址
const getBaseURL = async () => {
  const url = await ajax.getURL()
  baseURL.value = url
}

// 发起请求
const initiate = () => {
  request.value = ajax('api.do', {
    api: 'mtop.common.getTimestamp',
  })
    .then(({ data }) => data)
    .catch(({ config, ...error }) => error)
    .then(res => {
      result.value = JSON.stringify(res, null, 4)
    })
}

// 中断请求
const abort = () => {
  request?.abort()
}

onMounted(() => {
  getBaseURL()
  setTimeout(() => guideRef.value?.start(), 300) // 开启操作引导
})
</script>

<template>
  <view class="page">
    <view id="guide-2" class="preview" data-guide-text="请求结果">{{ result }}</view>

    <popup arrow drag-placeholder :stowed-height="350">
      <view id="guide-0" class="button" data-guide-text="点击请求服务端数据" @tap="initiate">
        发起请求
      </view>
      <view id="guide-1" class="button" data-guide-text="发起请求后可点击中断请求" @tap="abort">
        中断请求
      </view>
      <view class="text">接口根地址：{{ baseURL }}</view>
    </popup>

    <guide ref="guideRef" :total="3" />
  </view>
</template>

<style>
.page {
  padding-top: 30rpx;
  font-size: 28rpx;
  line-height: 1.15;
}

.preview {
  border-radius: 10rpx;
  margin: 0 30rpx;
  min-height: 16em;
  padding: 20rpx;
  background-color: #f5f5f5;
  color: #666666;
  font-size: 24rpx;
  white-space: break-spaces;
  line-height: 1.6;
}

.popup::v-deep .popup-drawer {
  box-shadow: 0rpx 0rpx 20rpx rgba(0, 0, 0, 0.2);
  padding: 30rpx;
  box-sizing: border-box;
}

.button {
  background: #376fee;
  box-shadow: 0 10rpx 20rpx rgba(10, 43, 245, 0.2);
  border-radius: 10rpx;
  color: #ffffff;
  font-size: 30rpx;
  text-align: center;
  line-height: 90rpx;
  transition: background-color 0.3s;
  height: 90rpx;
  margin-bottom: 30rpx;
}
.button:active {
  background-color: rgba(55, 111, 238, 0.75);
}

.text {
  color: #999999;
  padding: 0 10rpx;
}
</style>
