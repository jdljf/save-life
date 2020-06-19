<template>
  <div class="sl-tabs">
    <div class="sl-tabs__tabs-wrapper"
         ref="tabsContent">
      <div class="tabs-wrapper__header"
           v-for="(tab, i) in tabs"
           :id="`tab-header-${tab.value}`"
           @click="changeTab(tab.value)">
        {{tab.$slots.label || tab.label}}
      </div>
    </div>
    <div class="sl-tabs__border"
         :style="borderStyle">
      <span :style="barStyle"
            class="border__bar">
      </span>
    </div>
    <div class="sl-tabs__tabs-content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SlTabs',
  props: {
    value: {
      type: String,
      default: ""
    },
    activeValue: {
      type: String,
      default: ""
    },
    borderColor: {
      type: String,
      default: "#E4E7ED"
    },
    keepAlive: {
      type: Boolean,
      default: false
    }
  },
  computed: {
    borderStyle () {
      return {
        background: this.borderColor
      }
    },
    barStyle () {
      return {
        transform: `translateX(${this.barLeft}px)`,
        width: this.barWidth + "px"
      }
    }
  },
  data () {
    return {
      currentValue: this.value || this.activeValue,
      tabs: [],
      barLeft: 0,
      barWidth: 0,
    }
  },
  mounted () {
    this.calcPaneInstances()
    if (this.tabs.length > 0) {
      this.setCurrentValue(this.tabs[0].value)
    }
    this.barToActive()
  },
  methods: {
    calcPaneInstances (isForceUpdate = false) {
      console.log(this.$slots);

      if (this.$slots.default) {
        const tabSlots = this.$slots.default.filter(vnode => vnode.tag &&
          vnode.componentOptions && vnode.componentOptions.Ctor.options.name === 'SlTabItem');
        const tabs = tabSlots.map(({ componentInstance }) => componentInstance);

        this.tabs = tabs
      }
    },
    barToActive () {
      let that = this
      let tabsDoms = this.$refs.tabsContent;

      // 找到对应dom的索引
      this.$nextTick(() => {
        this.activeIndex = that.tabs.findIndex(d => {
          return d.id === `tab-item-${that.currentValue}`
        })

        if (that.activeIndex > 0) {
          that.barLeft = 0
          for (let i = 0; i <= that.activeIndex; i++) {
            const item = tabsDoms.children[i]
            let itemStyle = getComputedStyle(item)

            if (i < that.activeIndex) {
              that.barLeft += parseFloat(itemStyle.paddingLeft) + parseFloat(itemStyle.width) + parseFloat(itemStyle.paddingRight)
            }
            if (i == that.activeIndex) {
              that.barWidth = parseFloat(itemStyle.width)
              that.barLeft += parseFloat(itemStyle.paddingLeft)
            }
          }
        }
        else if (that.activeIndex === 0) {
          const item = tabsDoms.children[0]
          let itemStyle = getComputedStyle(item)
          that.barWidth = parseFloat(itemStyle.width)
          that.barLeft = 0
        }
      })
    },
    setCurrentValue (value) {
      this.currentValue = value
      this.$emit('input', value);
    },
    changeTab (val) {
      this.setCurrentValue(val)
      this.barToActive()
      this.$emit('tab-click', val)
    }
  }
}
</script>

<style scoped lang="scss">
.sl-tabs {
  font-size: 14px;
  color: #444;
  .sl-tabs__tabs-wrapper {
    white-space: pre;
    overflow-x: auto;
    .tabs-wrapper__header {
      display: inline-block;
      padding: 0 14px;
      min-width: 50px;
      cursor: pointer;
      white-space: nowrap;
      text-align: center;
    }
    .tabs-wrapper__header:first-child {
      padding-left: 0;
    }
    .tabs-wrapper__header:last-child {
      padding-right: 0;
    }
  }
  .sl-tabs__border {
    position: relative;
    width: 100%;
    height: 3px;
  }
  .border__bar {
    position: absolute;
    height: 3px;
    left: 0;
    top: 0;
    background: #409eff;
    transition: transform 0.3s cubic-bezier(0.645, 0.045, 0.355, 1);
  }
}
</style>
