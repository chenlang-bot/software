<template>
  <view class="container">
    <!-- 导航栏 -->
    <view class="nav-bar">
      <text class="nav-title">小说游戏推荐</text>
      <text class="nav-right" @click="goList">更多</text>
    </view>
    
    <!-- 轮播图 -->
    <swiper class="swiper" indicator-dots autoplay interval="3000">
      <swiper-item v-for="(item, index) in banners" :key="index">
        <image :src="item.image" mode="aspectFill"></image>
      </swiper-item>
    </swiper>
    
    <!-- 列表内容 -->
    <view class="news-list">
      <view class="news-item" v-for="item in newsList" :key="item.id" @click="goDetail(item.id)">
        <image :src="item.image" mode="aspectFill"></image>
        <view class="news-info">
          <text class="news-title">{{ item.title }}</text>
          <text class="news-desc">{{ item.desc }}</text>
          <text class="news-date">{{ item.date }}</text>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      banners: [],
      newsList: []
    }
  },
  onLoad() {
    this.loadData()
  },
  methods: {
    loadData() {
      // 模拟异步加载数据
      setTimeout(() => {
        this.banners = [
          { image: '/static/atri.png' },
          { image: '/static/ct.jpg' },
          { image: '/static/xt.jpg' },
		  { image: '/static/qt.jpg' }
        ]
        
        this.newsList = [
          { id: 1, title: '《亚托莉 我挚爱的时光》', desc: '跨越机械与人类的永恒羁绊', date: '2025-05-20', image: '/static/atri.png' },
          { id: 2, title: '《我想吃掉你的胰脏》', desc: '脆弱生命中绽放的纯美之恋', date: '2025-05-19', image: '/static/yz.jpg' },
          { id: 3, title: '《通往夏天的隧道 再见的出口》', desc: '穿越时空的青春告别之旅', date: '2025-05-18', image: '/static/xt.jpg' },
          { id: 4, title: '《在昨日的春天里等待你》', desc: '循环时光中的命运救赎', date: '2025-05-17', image: '/static/ct.jpg' },
          { id: 5, title: '《琥珀之秋 零秒之旅》', desc: '停滞时间中的记忆碎片', date: '2025-05-16', image: '/static/qt.jpg' },
          { id: 6, title: '《仿造品与绚烂多彩的灰》', desc: '虚拟与现实交织的人性考验', date: '2025-05-15', image: '/static/fzp.jpg' }
        ]
      }, 800)
    },
    
    goList() {
      uni.navigateTo({
        url: '/pages/list/list'
      })
    },
    
    goDetail(id) {
      uni.navigateTo({
        url: `/pages/detail/detail?id=${id}`
      })
    }
  }
}
</script>

<style>
.container {
  padding-top: 44px; /* 导航栏高度 */
  background-color: #f8f8f8;
}

.nav-bar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  height: 44px;
  line-height: 44px;
  background-color: #fff;
  border-bottom: 1px solid #eee;
  padding: 0 15px;
  display: flex;
  justify-content: space-between;
  z-index: 999;
}

.nav-title {
  font-size: 18px;
  font-weight: bold;
}

.nav-right {
  color: #007aff;
}

.swiper {
  height: 200px;
}

.swiper image {
  width: 100%;
  height: 100%;
}

.news-list {
  padding: 15px;
}

.news-item {
  display: flex;
  padding: 10px 0;
  border-bottom: 1px solid #eee;
}

.news-item image {
  width: 100px;
  height: 75px;
  border-radius: 4px;
}

.news-info {
  flex: 1;
  margin-left: 10px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.news-title {
  font-size: 16px;
  font-weight: bold;
  line-height: 1.3;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.news-desc {
  font-size: 12px;
  color: #666;
  margin-top: 5px;
  display: -webkit-box;
  -webkit-line-clamp: 1;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.news-date {
  font-size: 10px;
  color: #999;
  margin-top: 5px;
}
</style>