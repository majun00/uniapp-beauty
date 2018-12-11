<template>
  <div>
		<div class='query'>
			<p class='query-msg'>输入知乎问题网址后爬取图片,如下</p>
			<input class='query-input' type="text" :placeholder="cUrl" v-model="cUrl">
			<div class="btn" @click='init()'>确 认</div>
		</div>
    <div
      class="card"
      v-for="(item, index) in items"
      :key='index'
    >
      <div @click='onItemClick(item.url)'>
        <img
          class="image"
          :src="`http://47.110.44.176:3000/img?url=${item}`"
          alt=""
        >        <img
          class="image"
          :src="`${item}`"
          alt=""
        >
        <!-- <div class="title">{{item.title}}</div> -->
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [],
      hidden: false,
      mCurrentPage: 1,
      mUrl: [],
      mDesc: [],
      mWho: [],
      mTimes: [],
      mTitles: [],
      // cUrl: 'https://www.zhihu.com/question/29024583',
      cUrl: 'https://s.weibo.com/weibo?q=%23%E4%BD%A0%E6%8B%8D%E7%9A%84%E5%93%AA%E5%BC%A0%E7%9B%B8%E7%89%87%E6%9C%80%E6%9C%89%E7%88%B1%23'
    }
  },
  mounted() {
    // this.init(this.mCurrentPage)
  },
  onReachBottom() {
    this.hidden = false
    this.init(this.mCurrentPage + 1)
  },
  methods: {
    init(pageIndex=1) {
      const that = this

      wx.showToast({
        title: '加载中',
        icon: 'loading'
      });

      const data = {
        pageIndex: pageIndex,
        url: this.cUrl
      }

      wx.request({
        // url: 'http://47.110.44.176:3000',
        url: 'http://localhost:3000',
        method: 'POST',
        data: data,
        success: function (res) {
          that.items.push(...res.data.list)
          wx.hideToast();
        }
      });

    },

    onItemClick(url) {
      // const that = this
      // wx.previewImage({
      //   current: url,
      //   urls: that.mUrl
      // })
    },
  },
}
</script>

<style scoped>
.card {
  border: 2px solid #ffffff;
  border-radius: 5px;
  background-color: #ffffff;
  box-shadow: 0px 5px 1px #cccccc;
  margin: 8px;
  position: relative;
}

.loadmore {
  border: 0px solid #ffffff;
  border-radius: 5px;
  background-color: #ffffff;
  box-shadow: 0px 5px 1px #cccccc;
  margin: 8px;
}

.image {
  width: 100%;
  /* height: 240px; */
  /* object-fit: cover; */
}

.query-msg {
  padding: 10px 0;
	padding-left: 4px;
  font-size: 14px;
	color: #999;
}
.query-input{
	border-top: 1px solid #e8e8e8;
	border-bottom: 1px solid #e8e8e8;
	padding: 4px;
	font-size: 14px;
}
.btn{
	margin: 10px 0;
	padding: 10px;
	font-size: 16px;
	color: #fff;
	background-color: #666;
	text-align: center;
}
</style>
