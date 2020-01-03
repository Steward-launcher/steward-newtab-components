<template>
    <div class="sp-shortcuts">
        <span class="sp-shortcut-item" v-for="(item, index) in shortcuts"
            :key="index" @click="handleShortcutClick(item)">
            {{ item }}&nbsp;
        </span>
    </div>
</template>
<script>
export default {
    name: 'shortcuts',

    data() {
        return {
            shortcuts: this.getShortcuts()
        };
    },

    methods: {
        getShortcuts() {
            const shortcutsConfig = this.$root.config.general.shortcuts;
            let arr = [];

            [0, 1, 2, 3, 4, 5, 6, 7, 8, 9].forEach(index => {
                const conf = shortcutsConfig[`pageboxShortcut_${index}`];

                if (conf.cmd) {
                    arr.push(conf.cmd);
                }
            });

            return arr.slice(0, 5);
        },
        handleShortcutClick(item) {
            if (item) {
                window.stewardApp.applyCommand(item);
            }
        }
    }
} 
</script>
<style>
.sp-shortcuts {
  display: flex;
}

.sp-shortcuts .sp-shortcut-item {
  display: block;
  position: relative;
  margin-right: 15px;
  font-size: 14px;
  color: #fff;
  font-weight: 100;
  cursor: pointer;
  opacity: 1;
  transition: all .2s;
}

.sp-shortcuts .sp-shortcut-item:hover {
  opacity: 0.7;
}

.sp-shortcuts .sp-shortcut-item::after {
  position: absolute;
  right: -5px;
  top: 0;
  content: '|';
  transform: scale(0.7);
}

.sp-shortcuts .sp-shortcut-item:last-child::after {
  display: none;
}
</style>
