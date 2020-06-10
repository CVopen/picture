<template>
    <scroll-view class="recommend_view" @scrolltolower="handleTolower" scroll-y v-if="recommends.length > 0">
        <!-- 推荐开始 -->
        <view class="recommend_wrap">
            <view class="recommend_item" v-for="item in recommends" :key="item.id">
                <image :src="item.thumb" mode="widthFix"></image>
            </view>
        </view>
        <!-- 推荐结束 -->

        <!-- 月份开始 -->
        <view class="monthes_wrap">
            <view class="monthes_title">
                <view class="monthes_title_info">
                    <view class="monthes_info">
                        <text>{{monthes.DD}} /</text>
                        {{monthes.MM}} 月
                    </view>
                    <view class="monthes_text">{{monthes.title}}</view>
                </view>
                <view class="monthes_title_more">更多 ></view>
            </view>
            <view class="monthes_content">
                <view class="monthes_item" v-for="item in monthes.items" :key="item.id">
                  <image mode="aspectFill" :src="item.thumb + item.rule.replace('$<Height>', 360)"></image>
                </view>
            </view>
        </view>
        <!-- 月份结束 -->

        <!-- 热门开始 -->
        <view class="hots_wrap">
          <view class="hots_title">
            <text>热门</text>
          </view>
          <view class="hots_content">
            <view class="hot_item"
            v-for="item in hots"
            :key="item.id"
            >
              <image mode="widthFix" :src="item.thumb"></image>
            </view>
          </view>
        </view>
        <!-- 热门结束 -->
    </scroll-view>
</template>

<script>
import moment from 'moment'
export default {
    data() {
        return {
            // 推荐列表
            recommends: [],
            // 月份
            monthes: [],
            // 热门
            hots: [],
            // 请求的参数
            params: {
                // 获取几条
                limit: 30,
                // 关键字
                order: 'hot',
                // 跳过几条
                skip: 0
            },
            // 是否还有下一页
            hasMore: true
        }
    },
    mounted() {
        this.getList()
    },
    methods: {
      // 获取数据
      async getList(){
        const { data: res } = await this.request({
            url: 'http://157.122.54.189:9088/image/v3/homepage/vertical',
            data: this.params
        })
        // 判断是否存在下一页数据
        if (res.res.vertical.length === 0) {
          this.hasMore = false
          return
        }

        if (this.recommends.length === 0) {
          this.recommends = res.res.homepage[1].items
          this.monthes = res.res.homepage[2]
          // 将时间戳 改成 18号/月 moment.js
          this.monthes.MM = moment(this.monthes.stime).format('MM')
          this.monthes.DD = moment(this.monthes.stime).format('DD')
          // 获取热门数据的列表
        }
        
        this.hots = [...this.hots, ...res.res.vertical] 
      },
      // 滚动条触底事件
      handleTolower() {
        if (this.hasMore) {
          this.params.skip += this.params.limit
          this.getList()
        } else {
          uni.showToast({
            title: '没有数据了',
            icon: 'none'
          })
        }

      }
    },
}
</script>
<style lang="scss" scoped>
.recommend_view{
  height: calc(100vh - 36px);
}
.recommend_wrap {
    display: flex;
    flex-wrap: wrap;
    .recommend_item {
        width: 50%;
        border: 5rpx solid #fff;
    }
}
.monthes_wrap {
    .monthes_title {
        display: flex;
        justify-content: space-between;
        padding: 20rpx;
        color: $color;
        .monthes_title_info {
            font-size: 30rpx;
            font-weight: 600;
            display: flex;
            .monthes_info {
                margin-right: 30rpx;
                text {
                    font-size: 36rpx;
                }
            }
            .monthes_text {
                color: #666;
                font-size: 34rpx;
            }
        }
        .monthes_title_more {
            font-size: 34rpx;
        }
    }
    .monthes_content {
        display: flex;
        flex-wrap: wrap;
        .monthes_item {
          width: 33.3%;
          border: 5rpx solid #fff;
        }
    }
}
.hots_wrap{
  .hots_title{
    padding: 20rpx;
    text{
      border-left: 10rpx solid $color;
      font-size: 34rpx;
      font-weight: 600;
    }
  }
  .hots_content{
    display: flex;
    flex-wrap: wrap;
    .hot_item{
      width: 33.3%;
      border: 5rpx solid #fff;
      image{}
    }
  }
}
</style>
