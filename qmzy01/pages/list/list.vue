<template>
  <view class="container">
    <!-- 导航栏 -->
    <view class="nav-bar">
      <text class="nav-back" @click="goBack">◀</text>
      <text class="nav-title">小说游戏列表</text>
      <text class="nav-right"></text>
    </view>
    
    <!-- 搜索框 -->
    <view class="search-box">
      <input type="text" placeholder="搜索小说" v-model="keyword" @confirm="searchNews" />
      <text class="search-btn" @click="searchNews">搜索</text>
    </view>
    
    <!-- 分类标签 -->
    <view class="tags">
      <text class="tag-item" :class="{ active: currentTag === 'all' }" @click="changeTag('all')">全部</text>
      <text class="tag-item" :class="{ active: currentTag === '科幻' }" @click="changeTag('科幻')">科幻</text>
      <text class="tag-item" :class="{ active: currentTag === '纯爱' }" @click="changeTag('纯爱')">纯爱</text>
      <text class="tag-item" :class="{ active: currentTag === '奇幻' }" @click="changeTag('奇幻')">奇幻</text>
      <text class="tag-item" :class="{ active: currentTag === '悬疑' }" @click="changeTag('悬疑')">悬疑</text>
      <text class="tag-item" :class="{ active: currentTag === '治愈' }" @click="changeTag('治愈')">治愈</text>
    </view>
    
    <!-- 小说列表 -->
    <view class="news-list">
      <view class="news-item" v-for="item in filteredNewsList" :key="item.id" @click="goDetail(item.id)">
        <view class="news-header">
          <text class="news-title">{{ item.title }}</text>
          <text class="news-tag">{{ item.tag }}</text>
        </view>
        <text class="news-desc">{{ item.desc }}</text>
        <view class="news-meta">
          <text class="news-date">{{ item.date }}</text>
          <text class="news-source">{{ item.source }}</text>
        </view>
      </view>
    </view>
    
    <!-- 加载更多 -->
    <view class="load-more" v-if="!isLoading && hasMore" @click="loadMore">
      点击加载更多
    </view>
    <view class="loading" v-else-if="isLoading">
      加载中...
    </view>
    <view class="no-more" v-else>
      已加载全部内容
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      keyword: '',
      currentTag: 'all',
      allNewsList: [],
      newsList: [],
      page: 1,
      pageSize: 10,
      isLoading: false,
      hasMore: true
    }
  },
  computed: {
    filteredNewsList() {
      // 根据标签和关键词过滤
      return this.newsList.filter(item => {
        const tagMatch = this.currentTag === 'all' || item.tag === this.currentTag
        const keywordMatch = !this.keyword || item.title.includes(this.keyword)
        
        return tagMatch && keywordMatch
      })
    }
  },
  onLoad() {
    this.loadNews()
  },
  methods: {
    loadNews() {
      if (this.isLoading) return
      
      this.isLoading = true
      
      // 模拟异步加载数据
      setTimeout(() => {
        if (this.page === 1) {
          this.allNewsList = this.generateNews()
        }
        
        // 分页处理
        const start = (this.page - 1) * this.pageSize
        const end = start + this.pageSize
        this.newsList = [...this.newsList, ...this.allNewsList.slice(start, end)]
        
        // 判断是否还有更多数据
        this.hasMore = end < this.allNewsList.length
        this.page++
        this.isLoading = false
      }, 800)
    },
    
    loadMore() {
      if (!this.isLoading && this.hasMore) {
        this.loadNews()
      }
    },
    
    searchNews() {
      // 重置分页并搜索
      this.page = 1
      this.newsList = []
      this.hasMore = true
      this.loadNews()
    },
    
    changeTag(tag) {
      // 切换标签并重置分页
      this.currentTag = tag
      this.page = 1
      this.newsList = []
      this.hasMore = true
      this.loadNews()
    },
    
    goBack() {
      uni.navigateBack()
    },
    
    goDetail(id) {
      uni.navigateTo({
        url: `/pages/detail/detail?id=${id}`
      })
    },
    
    generateNews() {
      const sources = ['小说游戏情报站', 'ACG研究院', '虚拟世界观察', '二次元研究社']
      
      // 小说游戏数据
      return [
        { id: 1, title: '《亚托莉 我挚爱的时光》', tag: '科幻', desc: '在人类与机械共生的未来，一段跨越种族的禁忌之恋。'},
        { id: 2, title: '《我想吃掉你的胰脏》', tag: '纯爱', desc: '身患绝症的少女与冷漠少年，共同谱写生命最后的乐章。' },
        { id: 3, title: '《通往夏天的隧道 再见的出口》', tag: '奇幻', desc: '神秘隧道连接着过去与未来，少年少女能否改变注定的离别？'},
        { id: 4, title: '《在昨日的春天里等待你》', tag: '悬疑', desc: '被困在时间循环中的少年，必须解开谜题拯救青梅竹马。' },
        { id: 5, title: '《琥珀之秋 零秒之旅》', tag: '治愈', desc: '时间停止的小镇上，少年与少女寻找恢复时间流动的方法。'},
        { id: 6, title: '《仿造品与绚烂多彩的灰》', tag: '科幻', desc: '在虚拟现实技术高度发达的世界，探索真实与虚假的边界。' }
      ].map(item => {
        const randomSource = sources[Math.floor(Math.random() * sources.length)]
        const randomDate = `2025-05-${Math.floor(Math.random() * 25 + 1).toString().padStart(2, '0')}`
        
        return {
          ...item,
          date: randomDate,
          source: randomSource
        }
      })
    }
  }
}
</script>

<style>
.container {
  padding-top: 44px;
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

.nav-back {
  width: 30px;
  text-align: left;
}

.nav-title {
  font-size: 18px;
  font-weight: bold;
}

.search-box {
  display: flex;
  padding: 10px 15px;
  background-color: #fff;
  border-bottom: 1px solid #eee;
}

.search-box input {
  flex: 1;
  height: 32px;
  line-height: 32px;
  background-color: #f5f5f5;
  border-radius: 16px;
  padding: 0 15px;
  font-size: 14px;
}

.search-btn {
  width: 60px;
  height: 32px;
  line-height: 32px;
  text-align: center;
  color: #007aff;
  font-size: 14px;
}

.tags {
  display: flex;
  flex-wrap: wrap;
  padding: 10px 15px;
  background-color: #fff;
  border-bottom: 1px solid #eee;
}

.tag-item {
  padding: 5px 10px;
  margin-right: 10px;
  margin-bottom: 10px;
  border-radius: 15px;
  background-color: #f5f5f5;
  font-size: 14px;
}

.tag-item.active {
  background-color: #007aff;
  color: #fff;
}

.news-list {
  padding: 0 15px;
}

.news-item {
  padding: 15px 0;
  border-bottom: 1px solid #eee;
}

.news-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 8px;
}

.news-title {
  font-size: 16px;
  font-weight: bold;
  line-height: 1.3;
  max-width: 80%;
}

.news-tag {
  font-size: 12px;
  color: #fff;
  background-color: #ff6600;
  padding: 2px 5px;
  border-radius: 3px;
}

.news-desc {
  font-size: 14px;
  color: #666;
  line-height: 1.5;
  margin-bottom: 8px;
}

.news-meta {
  display: flex;
  font-size: 12px;
  color: #999;
}

.news-date {
  margin-right: 15px;
}

.load-more, .loading, .no-more {
  text-align: center;
  padding: 15px 0;
  color: #666;
  font-size: 14px;
}
</style>