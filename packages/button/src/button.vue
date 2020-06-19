<template>
  <button @click="handleClick"
          :class="classObj"
          :disabled="disabled"
          :type="realyType"
          :style="{
            color: this.color,
            backgroundColor: this.bgColor
          }"
          class="sl-button">
    <span>
      <slot name="front"></slot>
      <slot></slot>
      <slot name="behind"></slot>
    </span>
  </button>
</template>

<script>
export default {
  props: {
    size: {
      type: String,
      default: "normal"
    },
    disabled: {
      type: Boolean,
      default: false
    },
    "native-type": {
      type: String,
      default: "button"
    },
    color: {
      type: String,
      default: ""
    },
    bgColor: {
      type: String,
      default: ""
    }
  },
  computed: {
    classSize () {
      if (["mini", "small"].indexOf(this.size) >= 0) {
        return `sl-button__size-${this.size}`
      }
      else return "sl-button__size-normal"
    },
    classObj () {
      let obj = {
        'is-disabled': this.disabled
      }
      obj[this.classSize] = true

      return obj
    },
    realyType () {
      if (["reset", "submit"].indexOf(this["nativeType"]) >= 0) {
        return this["nativeType"]
      }
      else return "button"
    }
  },
  data () {
    return {

    }
  },
  methods: {
    handleClick () {
      this.$emit("click")
    }
  }
}
</script>

<style lang="scss" scoped>
.sl-button {
  display: inline-block;
  line-height: 1;
  white-space: nowrap;
  cursor: pointer;
  background: #fff;
  color: #606266;
  margin: 0;
  padding: 8px 12px;
  border: 1px solid #dcdfe6;
  outline: none; //消除默认点击蓝色边框效果
  border-radius: 4px;
}
.sl-button.sl-button__size-normal {
  padding: 10px 20px;
  font-size: 14px;
  border-radius: 4px;
}
.sl-button.sl-button__size-mini {
  padding: 7px 15px;
  font-size: 12px;
  border-radius: 3px;
}
.sl-button.sl-button__size-small {
  padding: 9px 15px;
  font-size: 12px;
  border-radius: 3px;
}
.sl-button.is-disabled {
  cursor: not-allowed;
  opacity: 0.6;
}
</style>
