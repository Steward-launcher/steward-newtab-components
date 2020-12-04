<template>
  <div class="sp-coinusd_p">
    <div class="sp-coin" v-if="info">
      <div class="sp-coin-symbol">{{title}}</div>
      <div class="sp-coin-price" :class="[isDown ? 'down' : 'up']">
        {{info.ticker[0]}}
        <svg class="sp-icon-price" v-if="isDown" viewBox="0 0 24 24" role="presentation" style="width: 0.9rem; height: 0.9rem;"><path d="M11,4H13V16L18.5,10.5L19.92,11.92L12,19.84L4.08,11.92L5.5,10.5L11,16V4Z"></path></svg>
        <svg class="sp-icon-price" v-else viewBox="0 0 24 24" role="presentation" style="width: 0.9rem; height: 0.9rem;"><path d="M13,20H11V8L5.5,13.5L4.08,12.08L12,4.16L19.92,12.08L18.5,13.5L13,8V20Z"></path></svg>
      </div>
    </div>
  </div>
</template>

<script>
module.exports = {
  props: ['pair'],
  data() {
    return {
      prevTicker: null,
      info: null
    }
  },

  computed: {
    title() {
      if (this.info) {
        return this.info.type.split('.')[1].toUpperCase()
      }
    },
    isDown() {
      if (this.prevTicker && this.info) {
        return this.prevTicker[0] > this.info.ticker[0]
      } else {
        return false
      }
    }
  },

  created() {
    this.init()
  },

  methods: {
    init() {
      setInterval(() => {
        this.fetchData()
      }, 1000)
    },
    fetchData() {
      this.$axios.get(`https://api.fmex.com/v2/market/ticker/${this.pair || 'BTCUSD_P'}`).then(({ data }) => {
        this.prevTicker = this.info && this.info.ticker
        this.info = data.data
      })
    }
  }
}
</script>

<style>
.sp-coin {
  padding: 4px 8px;
  background: rgba(0, 0, 0, 0.3);
}
.sp-coin .sp-coin-symbol {
  font-weight: 500;
  line-height: 1.56;
  font-size: 14px;
  color: rgba(255, 255, 255, 0.87);
}

.sp-coin .sp-coin-price {
  font-size: 16px;
}

.sp-coin .sp-coin-price .sp-icon-price {
  margin-left: 5px;
}

.sp-coin .sp-coin-price.down {
  color: #e96239;
}

.sp-coin .sp-coin-price.down .sp-icon-price {
  fill: #e96239;
}

.sp-coin .sp-coin-price.up {
  color: #00ab8d;
}

.sp-coin .sp-coin-price.up .sp-icon-price {
  fill: #00ab8d;
}
</style>