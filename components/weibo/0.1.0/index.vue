<template>
  <div class="sp-weibo" v-loading="loading" 
    customClass="sp-xq-loading">
    <div class="sp-weibo-tl" v-if="list && list.length">
      <div class="sp-weibo-item" v-for="item in list" :key="item.id" @click.stop="onItemClick($event, item)">
        <div class="sp-weibo-item-ulogo">
          <img :src="item.user.profile_image_url" alt="">
        </div>
        <div class="sp-weibo-item-desc" v-html="item.text"></div>
      </div>
    </div>
  </div>
</template>

<script>
module.exports = {
  name: 'weibo',
  props: ['uid', 'feature'],

  data() {
    return {
      loading: true,
      list: []
    };
  },

  mounted() {
    this.setup();
  },

  methods: {
    setup() {
      this.fetchData()
    },

    handleImgurl(desc) {
      if (desc) {
        const ret = desc.replace(/\/\/assets/g, 'https://assets')

        return ret
      } else {
        return desc
      }
    },
    fetchData() {
      this.$http.get('https://weibo.com/ajax/statuses/mymblog', {
        params: {
          uid: this.uid,
          page: 1,
          feature: this.feature || 0
        }
      }).then(({ data }) => {
        this.loading = false;
        const list = data.list
        list.forEach(item => {
          item.fold = true
        })
        this.list = list
      })
    },
    onItemClick(event, item) {
      const elem = event.target.closest('.sp-weibo-item-desc')

      item.fold = !item.fold
      if (item.fold) {
        this.$nextTick(() => {
          elem.scrollIntoView()
        })
      }
    }
  }
}
</script>

<style>
.sp-weibo {
  background: rgba(0, 0, 0, 0.5);
  height: 100%;
  overflow: auto;
  padding: 4px 8px;
  box-sizing: border-box;
}

.sp-weibo .el-loading-mask {
  background: transparent;
}

.sp-weibo .sp-weibo-item {
  display: flex;
  position: relative;
  padding: 8px 0 3px;
  color: #eee;
  font-size: 12px;
  border-bottom: 1px solid rgba(100, 100, 100, 0.4);
  cursor: pointer;
}

.sp-weibo .sp-weibo-item-ulogo {
  width: 24px;
  height: 24px;
  min-width: 24px;
}

.sp-weibo .sp-weibo-item-ulogo > img {
  display: block;
  border-radius: 50%;
  width: 100%;
  height: 100%;
}

.sp-weibo .sp-weibo-item-time {
  width: 50px;
}

.sp-weibo .sp-weibo-item-desc {
  margin-left: 10px;
  flex: 1;
}

.sp-weibo .sp-weibo-item-desc .ke_img {
  width: 100%;
}

.sp-weibo .sp-weibo-item-desc a {
  color: #06c;
  margin: 0 2px;
}
</style>