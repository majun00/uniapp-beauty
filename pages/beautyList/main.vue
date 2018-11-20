<template>
    <div>
        <div class="card" v-for="(item, index) in items" :key='index'>
            <div @click='onItemClick(item.url)'>
                <image class="image" mode="aspectFill" :src="item.url" />
                <div class="title">{{item.title}}</div>
            </div>
        </div>
    </div>
</template>

<script>
import Constant from '@/utils/constant.js'
export default {
    data() {
        return {
            items: [],
            hidden: false,
            mCurrentPage: 0,
            mUrl: [],
            mDesc: [],
            mWho: [],
            mTimes: [],
            mTitles: [],
        }
    },
    mounted() {
        this.init(this.mCurrentPage + 1)
    },
    onReachBottom() {
        this.hidden = false
        this.init(this.mCurrentPage + 1)
    },
    methods: {
        init(targetPage) {
            const that = this

            wx.showToast({
                title: '加载中',
                icon: 'loading'
            });

            wx.request({
                url: Constant.GET_MEIZHI_URL + targetPage,
                header: {
                    "Content-Type": "application/json"
                },
                success: function (res) {
                    if (res == null ||
                        res.data == null ||
                        res.data.results == null ||
                        res.data.results.length <= 0) {
                        return;
                    }

                    for (var i = 0; i < res.data.results.length; i++) {
                        that.bindData(res.data.results[i]);
                    }

                    var itemList = [];
                    for (var i = 0; i < that.mUrl.length; i++) {
                        itemList.push({ url: that.mUrl[i], desc: that.mDesc[i], who: that.mWho[i], time: that.mTimes[i], title: that.mTitles[i] });
                    }

                    that.items = itemList
                    that.hidden = true
                    that.mCurrentPage = targetPage;
                    wx.hideToast();
                }
            });

        },

        bindData(itemData) {
            let url = itemData.url.replace("//ww", "//ws");
            let desc = itemData.desc;
            let who = itemData.who;
            let times = itemData.publishedAt.split("T")[0];

            this.mUrl.push(url);
            this.mDesc.push(desc);
            this.mWho.push(who);
            this.mTimes.push(times);
            this.mTitles.push(times + " — " + who);
        },

        onItemClick(url) {
            const that = this
            wx.previewImage({
                current: url,
                urls: that.mUrl
            })
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
  height: 240px;
}

.title {
  padding: 14px;
  font-size: 14px;
}
</style>
