<template>
  <view class="container">
    <!-- 导航栏 -->
    <view class="nav-bar">
      <text class="nav-back" @click="goBack">◀</text>
      <text class="nav-title">{{ detail ? detail.title : '详情页' }}</text>
      <text class="nav-right"></text>
    </view>
    
    <!-- 详情内容 -->
    <view class="detail-content" v-if="detail">
      <view class="detail-header">
        <text class="detail-title">{{ detail.title }}</text>
        <view class="detail-meta">
          <text class="detail-date">{{ detail.date }}</text>
          <text class="detail-source">{{ detail.source }}</text>
          <text class="detail-views">{{ detail.views }}阅读</text>
        </view>
      </view>
      
      <view class="detail-body" v-html="detail.content"></view>
      
      <!-- 相关推荐 -->
      <view class="related-news">
        <text class="related-title">相关推荐</text>
        <view class="related-item" v-for="item in relatedList" :key="item.id" @click="goDetail(item.id)">
          <text>{{ item.title }}</text>
          <text class="item-date">{{ item.date }}</text>
        </view>
      </view>
    </view>
    
    <!-- 加载中状态 -->
    <view class="loading" v-else>
      加载中...
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      detail: null,
      relatedList: []
    }
  },
  onLoad(options) {
    this.loadDetail(options.id)
  },
  methods: {
    loadDetail(id) {
      // 模拟异步加载详情数据
      setTimeout(() => {
        this.detail = this.generateDetail(parseInt(id))
        this.relatedList = this.generateRelatedNews()
      }, 1000)
    },
    
    goBack() {
      uni.navigateBack()
    },
    
    goDetail(id) {
      uni.navigateTo({
        url: `/pages/detail/detail?id=${id}`
      })
    },
    
    generateDetail(id) {
      const sources = ['小说游戏情报站', 'ACG研究院', '虚拟世界观察', '二次元研究社']
      const randomSource = sources[Math.floor(Math.random() * sources.length)]
      const randomDate = `2025-05-${Math.floor(Math.random() * 25 + 1).toString().padStart(2, '0')} 0${Math.floor(Math.random() * 9 + 1)}:${Math.floor(Math.random() * 59).toString().padStart(2, '0')}`
      
      // 根据不同ID生成相关内容
      let content = ''
      let title = ''
      let coverImage = `https://picsum.photos/600/300?random=${id + 100}`
      
      // 根据ID映射到具体小说游戏
      switch(id) {
        case 1:
          title = '《亚托莉 我挚爱的时光》'
          content = `
            <p>《亚托莉 我挚爱的时光》是一款由Front Wing和枕社联合制作的视觉小说游戏。</p>
            
            <p><strong>故事背景：</strong></p>
            <p>因为不明原因导致全球海平面上升之后的未来，因事故失去了一条腿的男主斑鸠夏生却选择了放弃城市，转而搬家回到海边去生活，因外婆的遗债缘故而打捞上来的高性能仿生人ATRI，一切故事也由此展开</p>
            
            <image src="/static/atri2.png" mode="aspectFit"></image>
            
            <p><strong>游戏特色：</strong></p>
            <ul>
              <li>细腻的情感描写，探讨了机械生命是否拥有“心灵”</li>
              <li>精美的角色设计和背景音乐，营造出温馨而略带忧伤的氛围</li>
              <li>多条剧情分支，不同选择将影响故事走向和结局</li>
              <li>深刻的主题思考：何为心灵？何为爱？</li>
            </ul>
            
            <p><strong>玩家评价：</strong></p>
            <p>"我愿守护这笑容，直到生命的尽头"</p>
			<img src="/static/atri3.jpg">
            
            
          `
          break
          
        case 2:
          title = '《我想吃掉你的胰脏》'
          content = `
            <p>《我想吃掉你的胰脏》是由住野夜原作、loundraw插画的日本轻小说改编的视觉小说游戏，讲述了身患绝症的少女与性格孤僻的少年之间短暂而深刻的羁绊。</p>
            
            <p><strong>故事背景：</strong></p>
            <p>没有名字的"我"在医院捡到了患有胰脏疾病的少女山内樱良的日记。樱良没有将自己的病情告诉任何人，而"我"成为了她唯一可以倾诉的对象。两人在相处过程中，樱良的乐观和热情逐渐融化了"我"内心的坚冰。</p>
            
            <image src="/static/yz2(1).jpg" mode="aspectFit"></image>
            
            <p><strong>小说特色：</strong></p>
            <ul>
              <li>以独特的视角讲述生命与死亡的主题</li>
              <li>真实而细腻的人物刻画，主角之间的互动自然而感人</li>
              <li>充满张力的剧情发展，从日常相处到最终告别的情感递进</li>
              <li>原声音乐与剧情完美融合，增强了情感共鸣</li>
            </ul>
            
            <p><strong>改编信息：</strong></p>
            <p>除了小说外，该作还被改编为电影、动画等多种形式，均获得了广泛好评。但还是非常推荐读小说，有很多细节受动画时常限制而没能刻画出了。</p>
            
            <p>这是一个关于生命、爱情和告别的故事，它会让你重新审视自己的生活，珍惜与重要之人相处的每一刻。</p>
          `
          break
          
        case 3:
          title = '《通往夏天的隧道 再见的出口》'
          content = `
            <p>《通往夏天的隧道 再见的出口》是由八目迷创作的轻小说，融合了青春、奇幻和悬疑元素，讲述了少年与少女通过神秘的浦岛隧道加速时间的故事。</p>
            
            <p><strong>故事背景：</strong></p>
            <p>在某个夏天的夜晚，少年塔野熏发现了一条能追寻过去的神秘隧道。在隧道中，他遇到了因意外去世的妹妹伶的缺失的遗物。为了找回妹妹，塔野熏与新来的转校生花城杏子一起探索隧道的秘密，却意外卷入了时间加速的漩涡之中。</p>
            
            <image src="/static/xt2.jpg" mode="aspectFit"></image>
            
            <p><strong>小说特色：</strong></p>
            <ul>
              <li>独特的时间加速设定，充满科幻色彩的青春故事</li>
              <li>精美的夏日场景描绘，营造出怀旧而神秘的氛围</li>
              <li>独特的故事走向，不断加速的时间，是否能再见到心目中期盼已久的家人</li>
              <li>深刻探讨了命运、选择和告别的主题</li>
            </ul>
            
            <p><strong>作者风格：</strong></p>
            <p>八目迷以其独特的叙事风格著称，擅长将日常与奇幻元素巧妙结合。在本作中，他通过时间加速的设定，探讨了青春的遗憾与成长，让读者更能带入其中感同身受。</p>
            
            <p>这是一场跨越时间的青春冒险，当夏天结束时，少年少女能否找到属于他们的出口？</p>
          `
          break
          
        case 4:
          title = '《在昨日的春天里等待你》'
          content = `
            <p>《在昨日的春天里等待你》是一款融合了悬疑、奇幻和恋爱元素的轻小说，讲述了少年的意识意外来到六天后，同时得知青梅竹马的哥哥意外去世的消息，在混乱的时间中被请求救救自己哥哥的故事。</p>
            
            <p><strong>故事背景：</strong></p>
            <p>主角船见奏江在高中春天的某一天，突然陷入了时间回溯。每当下午六点的铃声想起，他的意识就会回到前一天的下午六点。为拯救明里的哥哥以及解开时间回溯的真相，不断探索的同时也发现明里隐瞒的秘密.....</p>
            
            <image src="/static/ct2.jpg" mode="aspectFit"></image>
            
            <p><strong>游戏特色：</strong></p>
            <ul>
              <li>时间回溯与推理元素的完美结合，每次回溯都会获得新线索</li>
              <li>不断回溯的时间，不为人知的秘密，不断接近的那天</li>
              <li>精美的角色设计和场景，特别是樱花季节的视觉表现</li>
              <li>情感丰富的剧情，主角与青梅竹马之间深厚的羁绊</li>
            </ul>
            
            <p><strong>小说特色：</strong></p>
            <p>小说采用了独特的"时间回溯"系统，读者需要在每次时间回溯中收集线索，逐步解锁真相。不断追寻时间的疑点，以及青梅竹马隐藏的秘密。</p>
            
            <p>这是一个关于爱与救赎的故事，当时间回溯后发现救与不救成了最大的难题，当最初的请求和最后的祈求相互背道而驰时，该如何抉择？</p>
          `
          break
          
        case 5:
          title = '《琥珀之秋 零秒之旅》'
          content = `
            <p>《琥珀之秋 零秒之旅》是一款以时间停滞为背景的轻小说，讲述了少年与少女在时间停止的时空，从北海道徒步回东京寻找重新让时间流逝的故事。</p>
            
            <p><strong>故事背景：</strong></p>
            <p>在某个秋日，主角麦野发现自己所在城市的事物突然静止不动。除了他和神秘少女熊井光，所有人和事物都静止在那一刻。会想起自己的舅舅曾来到过这“琥珀般的世界”，为了恢复时间流动，两人从北海道徒步前往东京的故事，揭开隐藏在时间停滞背后的秘密。</p>
            
            <image src="/static/qt2.jpg" mode="aspectFit"></image>
            
            <p><strong>小说特色：</strong></p>
            <ul>
              <li>独特的时间停滞设定，创造出静谧的氛围</li>
              <li>精美的秋日场景描绘，配合静止不动的世界，如同琥珀般，壮观</li>
              <li>长时间的相处及一路上的相互帮助，在静止的世界，两人是对方唯一的依靠</li>
              <li>这是一场关于时间与记忆的旅程，当一切静止时，真正重要的东西才会浮现。在这个停滞的秋日，少年与少女能否找到属于他们的答案？</li>
            </ul>
            
            <p><strong>个人评价：</strong></p>
            <p>相较于前两本小说，这本的结局相对突兀和仓促，非常的掉读者的好感度，但小说的内容更多的是两人在互相扶持的过程中的心理变化和情感变化，从内容上来说这是一本合格的青春恋爱小说。</p>
            
            <p></p>
          `
          break
          
        case 6:
          title = '《仿造品与绚烂多彩的灰》'
          content = `
            <p>《仿造品与绚烂多彩的灰》是一本双视角出发，笨拙的少年不断追寻不变的少女的轨迹的科幻小说，探讨了真实与虚假、存在与意义的哲学问题。</p>
            
            <p><strong>故事简介：</strong></p>
            <p>有着蓝色眼眸的神秘少女，带着陈旧的相机，如同候鸟一般出现在人们眼前。与她相遇的笨拙少年一直追踪着她的轨迹。当时间、地点和人物都连接起来的时候，埋藏在少女心底的“那份悔意”，将强烈转动两人间命运的齿轮。</p>
            
            <image src="/static/fzp2.jpg" mode="aspectFit"></image>
            
            <p><strong>小说特色：</strong></p>
            <ul>
              <li>科幻与哲学的深度融合，探讨了AI意识、身份认同等前沿话题</li>
              <li>特殊的角色设定：笨拙的少年不断追寻少女的足迹却发现她一点没改变</li>
              <li>复杂的剧情架构，不同的视角，不断追寻突然出现又突然消失痕迹</li>
              <li>富有张力的角色关系，主角与各角色之间的互动充满悬念</li>
            </ul>
            
            <p><strong>读者探讨：</strong></p>
            <p>突然的相遇突然的别离，追寻脚步的同时发现少女的秘密，回到故事开始的地方，解答最后的疑问，享受最后的时光。</p>
            
            <p>小说美丽、澄净的同时又带着一点点苦涩。跨越着时间与空间，追逐着“爱”的二人，最终将对方命运的齿轮转动，迎来了最初也是最后的奇迹。。</p>
          `
          break
          
        default:
          title = `《未定义的世界 #${id}》`
          content = `
            <p>这是一个尚未被定义的世界，等待着玩家去探索和发现。也许有一天，这里会诞生属于你的故事。</p>
            <p>敬请期待更多内容更新！</p>
          `
      }
      
      return {
        id,
        title,
        date: randomDate,
        source: randomSource,
        views: Math.floor(Math.random() * 10000 + 1000),
        content
      }
    },
    
    generateRelatedNews() {
      const related = []
      const currentId = this.detail ? this.detail.id : 1
      
      // 获取同一主题的其他作品（随机选择3个）
      const sameThemeIds = [1, 2, 3, 4, 5, 6].filter(id => id !== currentId)
      for (let i = 0; i < 3 && i < sameThemeIds.length; i++) {
        const id = sameThemeIds[i]
        related.push({
          id,
          title: this.getNovelTitle(id),
          date: `2025-05-${Math.floor(Math.random() * 25 + 1).toString().padStart(2, '0')}`
        })
      }
      
      // 添加一个随机其他作品
      const otherId = (currentId % 6) + 1
      if (otherId !== currentId && !related.some(item => item.id === otherId)) {
        related.push({
          id: otherId,
          title: this.getNovelTitle(otherId),
          date: `2025-05-${Math.floor(Math.random() * 25 + 1).toString().padStart(2, '0')}`
        })
      }
      
      return related
    },
    
    // 辅助函数：获取小说标题
    getNovelTitle(id) {
      const titles = [
        '《亚托莉 我挚爱的时光》',
        '《我想吃掉你的胰脏》',
        '《通往夏天的隧道 再见的出口》',
        '《在昨日的春天里等待你》',
        '《琥珀之秋 零秒之旅》',
        '《仿造品与绚烂多彩的灰》'
      ]
      return titles[id - 1] || `《未定义的世界 #${id}》`
    }
  }
}
</script>

<style>
.container {
  padding-top: 44px;
  background-color: #fff;
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

.detail-content {
  padding: 15px;
}

.detail-title {
  font-size: 20px;
  font-weight: bold;
  line-height: 1.3;
  margin-bottom: 10px;
  display: block;
}

.detail-meta {
  display: flex;
  font-size: 12px;
  color: #999;
  margin-bottom: 15px;
}

.detail-date, .detail-source {
  margin-right: 15px;
}

.detail-body {
  font-size: 16px;
  line-height: 1.8;
  color: #333;
}

.detail-body p {
  margin-bottom: 15px;
}

.detail-body image {
  display: block;
  width: 100%;
  margin: 15px 0;
  border-radius: 4px;
}

.related-news {
  margin-top: 30px;
  padding-top: 15px;
  border-top: 1px solid #eee;
}

.related-title {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 10px;
  display: block;
}

.related-item {
  padding: 10px 0;
  border-bottom: 1px solid #eee;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.related-item text:first-child {
  max-width: 80%;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.item-date {
  font-size: 12px;
  color: #999;
}

.loading {
  text-align: center;
  padding: 50px 0;
  font-size: 16px;
  color: #666;
}
</style>