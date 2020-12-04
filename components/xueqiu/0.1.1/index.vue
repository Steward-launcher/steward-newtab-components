<template>
  <div class="sp-xueqiu" v-loading="loading" 
    customClass="sp-xq-loading">
    <div class="sp-xueqiu-tl" v-if="list && list.length">
      <div class="sp-xueqiu-item" v-for="item in list" :key="item.id" @click.stop="onItemClick($event, item)">
        <div class="sp-xueqiu-item-ulogo">
          <img :src="`https:${item.user.photo_domain}${item.user.profile_image_url.split(',')[0]}`" alt="">
        </div>
        <div class="sp-xueqiu-item-desc" v-if="item.fold" v-html="item.description"></div>
        <div class="sp-xueqiu-item-desc" v-else v-html="item.text"></div>
      </div>
    </div>
  </div>
</template>

<script>
module.exports = {
  name: 'xueqiu',
  props: ['user_id', 'type'],

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
      this.$http.get('https://xueqiu.com/v4/statuses/user_timeline.json', {
        params: {
          page: 1,
          user_id: this.user_id,
          type: this.type || 0
        }
      }).then(({ data }) => {
        this.loading = false;
        const list = data.statuses
        list.forEach(item => {
          item.fold = true
          item.description = this.handleImgurl(item.description)
        })
        this.list = list
      })
    },
    onItemClick(event, item) {
      const elem = event.target.closest('.sp-xueqiu-item-desc')

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
.sp-xueqiu {
  background: rgba(0, 0, 0, 0.5);
  height: 100%;
  overflow: auto;
  padding: 4px 8px;
  box-sizing: border-box;
}

.sp-xueqiu .el-loading-mask {
  background: transparent;
}

.sp-xueqiu .sp-xueqiu-item {
  display: flex;
  position: relative;
  padding: 8px 0 3px;
  color: #eee;
  font-size: 12px;
  border-bottom: 1px solid rgba(100, 100, 100, 0.4);
  cursor: pointer;
}

.sp-xueqiu .sp-xueqiu-item-ulogo {
  width: 24px;
  height: 24px;
  min-width: 24px;
}

.sp-xueqiu .sp-xueqiu-item-ulogo > img {
  display: block;
  border-radius: 50%;
  width: 100%;
  height: 100%;
}

.sp-xueqiu .sp-xueqiu-item-time {
  width: 50px;
}

.sp-xueqiu .sp-xueqiu-item-desc {
  margin-left: 10px;
  flex: 1;
}

.sp-xueqiu .sp-xueqiu-item-desc .ke_img {
  width: 100%;
}

.sp-xueqiu .sp-xueqiu-item-desc a {
  color: #06c;
  margin: 0 2px;
}
</style>