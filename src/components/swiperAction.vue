/**
  1 slot
  2 对外提供数据 滑动的方向
 */
<template>
    <view @touchstart="hanleTouchStart" @touchend="handleTouchEnd">
        <slot></slot>
    </view>
</template>
<script>
export default {
    data() {
        return {
            // 按下的时间
            startTime: 0,
            // 按下的坐标
            startX: 0,
            startY: 0
        }
    },
    methods: {
        // 用户按下屏幕
        hanleTouchStart(event) {
            // console.log('按下的' + event.changedTouches[0].clientX)
            // console.log('按下的' + event.changedTouches[0].clientY)
            this.startTime = Date.now()
            this.startX = event.changedTouches[0].clientX
            this.startY = event.changedTouches[0].clientY
        },
        handleTouchEnd(event) {
            // console.log('离开的' + event.changedTouches[0].clientX)
            // console.log('离开的' + event.changedTouches[0].clientY)
            let endTime = Date.now()
            const endX = event.changedTouches[0].clientX
            const endY = event.changedTouches[0].clientY

            // 判断按下的时长
            if (endTime - this.startTime > 2000) {
                return
            }

            // 滑动的方向
            let direction = ''
            // 先判断用户滑动的距离
            if (
                Math.abs(endX - this.startX) > 10 &&
                Math.abs(endY - this.startY) < 10
            ) {
                // 滑动方向
                direction = endX - this.startX > 0 ? 'right' : 'left'
            } else {
                return
            }

            // 用户做了一个合法的滑动操作
            this.$emit('swiperAction', { direction })
        }
    }
}
</script>
<style lang="scss" scoped>
</style>
