<template>
  <div :data-value="value"
       class="sl-checkbox-group">
    <slot></slot>
  </div>
</template>

<script>
export default {
  props: {
    value: { default: "" },
    disabled: { default: false },
  },
  created () {
    this.$on('handleChange', value => {
      this.$emit('change', value);
    });
  },
  methods: {
  }
}
</script>