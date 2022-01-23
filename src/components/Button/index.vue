<template>
  <button :class="`button${className ? ' ' + className : ''}`">
    <slot/>
  </button>
</template>

<script>
export default {
  name: "Button",
  props: {
    className: {
      type: String,
      default: null
    }
  }
}
</script>

<style lang="scss" scoped>
  .button {
    border: 0;
    background: none;
    padding: 5px;
  }
</style>
