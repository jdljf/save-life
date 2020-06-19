<template>
  <label class="sl-checkbox-group-item"
         :class="{'is-disabled': isDisabled}">
    <input type="checkbox"
           :checked="isChecked(item.value)"
           v-on:change="onChange($event)"
           :value="item.value"
           :id="'id' + item.value"
           class="sl-checkbox-group-item__input"
           ref="slCheckBox">
    </input>
    <slot :scoped="item"
          :checked="isChecked(item.value)">
    </slot>
  </label>
</template>

<script>
export default {
  props: {
    item: {
      type: Object,
      default: function () {
        return {}
      }
    },
    checked: {
      type: Boolean,
      default: false
    },
    disabled: {
      type: Boolean,
      default: false
    }
  },
  computed: {
    isDisabled () {
      return this.disabled || this.$parent.disabled
    },
    resultChecked (flag) {
      return this.isChecked(this.item.value)
    }
  },
  mounted () {
    if (this.checked) {
      let dom = this.$refs.slCheckBox
      dom.checked = "checked"
      this.$parent.mountedCheckedModel(this.item)
      this.isChecked(this.item.value)
    }
  },
  methods: {
    onChange (event) {
      if (this.isDisabled) return
      this.$parent.onChange(event, this.item)
    },
    isChecked (val) {
      return this.$parent.isChecked(val)
    }
  }
}
</script>

<style lang="scss" scoped>
.sl-checkbox-group-item {
  position: relative;
  display: inline-block;
  .sl-checkbox-group-item__input {
    opacity: 0;
    outline: none;
    position: absolute;
    margin: 0;
    z-index: -1;
    opacity: 0;
  }
}
.is-disabled {
  cursor: not-allowed;
}
</style>