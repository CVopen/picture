<template>
    <view @touchstart="hanleTouchStart" @touchend="handleTouchEnd">学习触屏事件</view>
</template>
<script>
/**
 * 1 给容器绑定两个触屏事件  touchstart 和 touchend
 * 2 用户按下屏库的事件
 *  1 记录用户按下屏幕的时间   Date.now() 1970 -1 -1 到现在的毫秒数
 *  2 记录用户按下屏幕的左边 x 和 y
 * 3 用户离开屏幕的事件
 *  1 记录用户离开屏幕的时间   Date.now()
 *  2 记录用户离开屏幕的坐标  x 和 y
 *  3 根据两个时间 运算 判断 用户按下屏幕时长是否合法
 *  4 根据两对坐标 判断距离是否合法
 */
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
            if (Math.abs(endX - this.startX) > 10) {
                // 滑动方向
                direction = endX - this.startX > 0 ? 'right' : 'left'
            } else {
                return
            }

            // 用户做了一个合法的滑动操作
            console.log(direction)
        }
    }
}
</script>
<style lang="scss" scoped>
view {
    width: 100%;
    height: 500rpx;
    background-color: aqua;
}
</style>
