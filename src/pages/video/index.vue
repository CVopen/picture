<template>
    <view>
        <view class="video_tab">
            <view class="video_tab_title">
                <view class="title_inner">
                    <uni-segmented-control
                        :current="current"
                        :values="items.map(v => v.title)"
                        @clickItem="onClickItem"
                        style-type="text"
                        active-color="#d4237a"
                    ></uni-segmented-control>
                </view>
                <view class="iconfont icon-sousuo"></view>
            </view>

            <view class="video_tab_content">视频</view>
        </view>
    </view>
</template>
<script>
import { uniSegmentedControl } from '@dcloudio/uni-ui'
export default {
    components: {
        uniSegmentedControl
    },
    data() {
        return {
            items: [
                { title: '推荐' },
                { title: '娱乐' },
                { title: '最新' },
                { title: '热门' },
                { title: '分类' }
            ],
            current: 0
        }
    },
    methods: {
        onClickItem(e) {
            if (this.current !== e.currentIndex) {
                this.current = e.currentIndex
            }
        }
    }
}
</script>
<style lang="scss" scpoed>
.video_tab {
    .video_tab_title {
        position: relative;
        .title_inner {
            width: 60%;
            margin: 0 auto;
        }
        .icon-sousuo {
            position: absolute;
            top: 50%;
            transform: translateY(-50%);
            right: 5%;
        }
    }
    .video_tab_content {
    }
}
</style>