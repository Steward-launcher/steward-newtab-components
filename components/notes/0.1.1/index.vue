<template>
  <div class="sp-notes">{{ note }}</div>
</template>

<script>
module.exports = {
  name: 'Notes',
  data() {
    return {
      note: ''
    }
  },

  methods: {
    async getOneNote() {
      const { notes = [] } = await window.stewardApp.browser.storage.sync.get('notes')

      if (notes && notes.length) {
        return notes[Math.floor(Math.random(notes.length) * notes.length)]
      } else {
        return ''
      }
    },

    async random() {
      const item = await this.getOneNote()

      if (item) {
        this.note = item.text
      }
    }
  },

  mounted() {
    this.random()
  }
}
</script>

<style>
  .sp-notes {
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
    color: #ddd;
  }
</style>