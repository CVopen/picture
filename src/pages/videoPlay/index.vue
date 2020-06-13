<template>
    <view class="video_play">
      <image :src="videoObj.img"></image>
        <!-- 工具栏开始 -->
        <view class="video_tool">
            <view @click="handleMuted" :class="['icon', muted ? 'icont' : '']">音</view>
            <view class="icon iconBtn">转
              <button open-type="share"></button>
            </view>
        </view>
        <!-- 工具栏结束 -->

        <!-- 视频开始 -->
        <view class="video_wrap">
            <video :src="videoObj.video" objectFit="fill" :muted="muted"></video>
        </view>
        <!-- 视频结束 -->

        <!-- 下载开始 -->
        <view class="download">
            <view class="download_btn" @click="handleDownload">下载高清</view>
        </view>
        <!-- 下载结束 -->
    </view>
</template>
<script>
export default {
  data(){
    return {
      videoObj: {},
      // 是否静音
      muted: false
    }
  },
    onLoad() {
        console.log(getApp().globalData.video)
        this.videoObj = getApp().globalData.video
    },
    methods: {
      // 开关声音
      handleMuted() {
        this.muted = !this.muted
      },
      // 下载视频
      async handleDownload () {

        await uni.showLoading({title: '下载中'})

        // 将远程文件 下载到小程序的内存中
        // 将内存中的文件 下载到本地上
        const {tempFilePath} = (await uni.downloadFile({
          url: this.videoObj.video
        }))[1]
        await uni.saveVideoToPhotosAlbum({
          filePath: tempFilePath
        })
        uni.hideLoading()
        await uni.showToast({
          title: '下载成功',
          icon: 'none'
        })
      }
    }
}
</script>
<style scoped lang="scss">
  .video_play{
    position: relative;
    image{
      position: absolute;
      width: 100vw;
      height: 100vh;
      z-index: -1;
      // css3 滤镜
      filter: blur(20px)
    }
    .video_tool{
      height: 80rpx;
      display: flex;
      justify-content: flex-end;
      .icon{
        width: 80rpx;
        height: 80rpx;
        font-size: 50rpx;
        border-radius: 40rpx;
        color: $color;
        background-color: rgba(0,0,0,.2);
        display: flex;
        justify-content: center;
        align-items: center;
        margin-right: 20rpx;
      }
      .icont{
        color: aqua;
      }
      .iconBtn{
        position: relative;
        button{
          position: absolute;
          width: 100%;
          height: 100%;
          opacity: 0;
          top: 0;
          left: 0;
        }
      }
    }
    .video_wrap{
      display: flex;
      justify-content: center;
      video{
        width: 360rpx;
        height: 600rpx;
      }
    }
    .download{
      display: flex;
      justify-content: center;
      margin-top: 30rpx;
      .download_btn{
        width: 360rpx;
        height: 80rpx;
        border-radius: 40rpx;
        display: flex;
        justify-content: center;
        align-items: center;
        color: #fff;
        border: 3rpx solid #fff;
        background-color: rgba(0,0,0,.4);
      }
    }
  }

</style>