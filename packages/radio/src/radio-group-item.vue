<template>
  <label class="sl-checkbox-group-item"
         :class="{'is-disabled': isDisabled}">
    <input type="radio"
           @change="handleChange"
           :value="item.value"
           v-model="model"
           :id="'id' + item.value"
           class="sl-checkbox-group-item__input"
           ref="slCheckBox">
    </input>
    <slot :scoped="item"
          :checked="isChecked">
      {{item.label}}
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
    model: {
      get () {
        return this.$parent.value
      },
      set (val) {
        this.$parent.$emit("input", val)
      }
    },
    isDisabled () {
      return this.disabled || this.$parent.disabled
    },
    isChecked () {
      return this.model === this.item.value
    }
  },
  mounted () {

  },
  methods: {
    handleChange () {
      this.$nextTick(() => {
        this.$parent.$emit('handleChange', this.model)
      });
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