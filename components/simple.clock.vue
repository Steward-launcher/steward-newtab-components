<template>
    <div class="clock">
        <div class="time">{{ time }}</div>
        <div class="date">{{ date }}</div>
    </div>    
</template>
<script>
module.exports = {
    name: 'clock',
    data() {
        return {
            time: this.$dayjs().format('HH:mm'),
            date: this.$dayjs().format('YYYY-MM-DD')
        };
    },

    created() {
        setInterval(() => {
            this.count();
        }, 1000);
    },

    methods: {
        count() {
            this.time = this.$dayjs().format('HH:mm');
            this.date = this.$dayjs().format('YYYY-MM-DD');
        }
    }
}
</script>
<style>
    .clock {
        position: relative;
    }

    .clock:hover .date {
      opacity: 1;
    }

    .clock .time {
        font-size: 80px;
        color: #fff;
        font-weight: 100;
    }

    .clock .date {
        position: absolute;
        bottom: -10px;
        right: 0.4em;
        text-align: right;
        font-size: 20px;
        color: #fff;
        font-weight: 100;
        opacity: 0;
        transition: all .2s;
    }
</style>

