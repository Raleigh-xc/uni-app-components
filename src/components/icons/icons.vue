<!-- 弃用 -->
<template>
  <text
    :style="{ color: color, 'font-size': size + 'px' }"
    class="iconfont"
    :class="'icon-' + type"
    @tap="handleTap"
  ></text>
</template>

<script>
export default {
  name: "Icons",
  props: {
    type: {
      type: String,
      default: ""
    },
    color: {
      type: String,
      default: "#333333"
    },
    size: {
      type: [Number, String],
      default: 16
    }
  },
  data() {
    return {};
  },
  methods: {
    handleTap() {
      this.$emit("tap");
    }
  }
};
</script>

<style lang="scss" scoped>
@import url("~@/static/fonts/iconfont.css");
</style>
