<template>
  <a class="sp-bookmark" :href="bookmark.url">{{ bookmark.title }}</a>
</template>

<script>
module.exports = {
  name: 'Bookmarks',
  data() {
    return {
      bookmark: {
        url: '',
        title: ''
      }
    }
  },

  methods: {
    async getOneBookmark() {
      const list = await window.stewardApp.browser.bookmarks.getRecent(2147483647)

      if (list && list.length) {
        return list[Math.floor(Math.random(list.length) * list.length)]
      } else {
        return
      }
    },

    async random() {
      const item = await this.getOneBookmark()

      if (item) {
        this.bookmark = item
      }
    }
  },

  mounted() {
    this.random()
  }
}
</script>

<style>
  .sp-bookmark {
    position: absolute;
    background: rgba(0, 0, 0, .3);
    padding: 10px;
    bottom: 0;
    font-size: 14px;
    width: 100%;
    overflow: hidden;
    text-align: center;
    text-overflow: ellipsis;
    white-space: nowrap;
    text-decoration: none;
    color: #ddd;
  }
</style>