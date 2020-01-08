<template>
  <div class="sp-fmzlog">
    <div v-if="latestLog" :class="['sp-fmzlog-item', `sp-fmzlog-${this.status}`]">BTC ATR: {{latestLog[6]}}</div>
  </div>
</template>

<script>
module.exports = {
  name: 'FMZLog',
  props: ['accessKey', 'secretKey', 'robotId'],

  data() {
    return {
      version: '1.0',
      logs: []
    };
  },

  computed: {
    latestLog() {
      if (this.logs && this.logs.Arr) {
        const value = this.logs.Arr[this.logs.Arr.length - 1]

        if (value) {
          value[6] = value[6].substr(0, 4)
          return value
        }
      }
    },
    status() {
      const latestLog = this.latestLog;
      let status = 'normal'

      if (latestLog) {
        const atr =  latestLog[6]

        if (atr > 12) {
          status = 'danger'
        } else if (atr > 8) {
          status = 'warning'
        } else if (atr < 4) {
          status = 'success'
        }
      }

      return status
    }
  },

  mounted() {
    this.fetchLog()
    setInterval(() => {
      this.fetchLog()
    }, 3000)
  },

  methods: {
    sign(params) {
      const { method, args, nonce } = params
      const str = [this.version, method, args, nonce, this.secretKey].join('|')

      return window.stewardApp.md5(str)
    },

    fetchLog() {
      const params = {
        method: 'GetRobotLogs',
        args: JSON.stringify([this.robotId, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]),
        nonce: Number(new Date())
      }

      const sign = this.sign(params)
      this.$axios.get('https://www.fmz.com/api/v1', {
        params: {
          version: this.version,
          access_key: this.accessKey,
          ...params,
          sign
        }
      }).then(({ data }) => {
        if (data.data) {
          this.logs = data.data.result.logs[0]
        }
      })
    }
  }
}
</script>

<style>
.sp-fmzlog {
  padding: 4px 8px;
  background: rgba(0, 0, 0, 0.3);
}
.sp-fmzlog-item {
  font-size: 14px;
  font-weight: 500;
  line-height: 1.56;
}

.sp-fmzlog-item.sp-fmzlog-normal {
  color: #303133;
}

.sp-fmzlog-item.sp-fmzlog-success {
  color: #67C23A;
}

.sp-fmzlog-item.sp-fmzlog-warning {
  color: #E6A23C;
}

.sp-fmzlog-item.sp-fmzlog-danger {
  color: #F56C6C;
}
</style>