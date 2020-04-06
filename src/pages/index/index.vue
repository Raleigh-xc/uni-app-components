<template>
  <view class="container">
    <uni-section style="margin-top:0px;" title="scroll-tabs" type="line"></uni-section>
    <d-scroll-tabs :data="list" :active="current" showPicker @tabChange="handleTabChange"></d-scroll-tabs>
    <uni-section title="icons" type="line"></uni-section>
    <text class="iconfont icon-all"></text>
    <text class="iconfont icon-integral"></text>
  </view>
</template>

<script>
import dScrollTabs from "@/components/scroll-tabs/scroll-tabs.vue";
// import dIcons from "@/components/icons/icons.vue";
import { uniSection } from "@dcloudio/uni-ui";
export default {
  components: {
    dScrollTabs,
    // dIcons,
    uniSection
  },
  data() {
    return {
      list: [],
      current: 0
    };
  },
  onLoad() {
    this.init();
  },
  methods: {
    init() {
      this.list = [
        {
          title: "基础信息"
        },
        {
          title: "备件BOM"
        },
        {
          title: "维保履历"
        },
        {
          title: "使用履历"
        },
        {
          title: "文档管理"
        },
        {
          title: "备注"
        }
      ];
    },
    handleTabChange(value) {
      this.current = value;
    }
  }
};
</script>

<style lang="scss" scoped>
.tabSelector {
  height: 40px;
  width: 40px;
  background: #999;
}
</style>>