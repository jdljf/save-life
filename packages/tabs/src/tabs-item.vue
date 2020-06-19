<template>
  <div class="sl-tabs-item"
       v-show="active"
       :id="id">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: "SlTabItem",
  props: {
    label: {
      type: String,
      required: true,
      default: ""
    },
    isDisabled: {
      type: Boolean,
      default: false
    },
    value: {
      type: String,
      required: true,
      default: ""
    },
    closeable: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      index: null
    }
  },
  computed: {
    active () {
      const active = this.$parent.currentValue === (this.value || this.index);
      if (active) {
        this.loaded = true;
      }
      return active;
    },
    id () {
      return `tab-item-${this.value}`
    }
  },
  methods: {
    changeTabsItem () {
      this.$parent.changeTab(this.value)
    }
  }
}
</script>

<style lang="scss" scoped>
</style>