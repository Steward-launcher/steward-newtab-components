<template>
  <div class="sp-wordcard">
    <div class="word-fields" v-if="word" @dblclick="showRandom">
      <div class="word-card-name word-field" @click="word.transVisible = true">{{word.name}}</div>
      <div class="word-card-trans word-field">
        <span v-if="word.trans && word.trans.length" class="word-trans-text" :class="{'is-visible': word.transVisible}">{{(word.trans || []).join(',')}}</span>
      </div>
    </div>
  </div>
</template>

<script>
module.exports = {
  name: "Wordcard",
  props: {
    extID: {
      type: String,
      default: "oegblnjiajbfeegijlnblepdodmnddbk"
    },
    interval: {
      type: Number,
      default: 30000
    }
  },
  data() {
    return {
      words: [],
      word: null
    };
  },
  methods: {
    getWords() {
      return new Promise(resolve => {
        chrome.runtime.sendMessage(
          this.extID,
          {
            action: "filter",
            query: ""
          },
          ({ data = [] }) => {
            resolve(data);
          }
        );
      });
    },
    random() {
      const words = this.words;

      if (words && words.length) {
        return {
          transVisible: false,
          ...words[Math.floor(Math.random(words.length) * words.length)]
        }
      } else {
        return null;
      }
    },
    showRandom() {
      this.word = this.random();
    },
    setup() {
      this.getWords().then(words => {
        this.words = words;
        this.showRandom();
      });

      setInterval(() => {
        this.showRandom();
      }, this.interval);
    }
  },
  created() {
    this.setup();
  }
};
</script>

<style>
.sp-wordcard {
  padding: 10px 15px;
  background: rgba(0, 0, 0, 0.3);
  color: rgba(255, 255, 255, 0.87);
  display: flex;
  flex-direction: column;
  height: 100%;
}

.sp-wordcard .word-fields {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex: 1;
}

.sp-wordcard .word-card-name {
  text-align: left;
  font-size: 16px;
  font-weight: 700;
  cursor: pointer;
}

.sp-wordcard .word-card-trans {
  font-size: 14px;
  text-align: left;
}

.sp-wordcard .word-trans-text {
  visibility: hidden;
}

.sp-wordcard .word-trans-text.is-visible {
  visibility: visible;
}
</style>