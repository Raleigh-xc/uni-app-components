<template>
  <view class="container">
    <scroll-view scroll-x scroll-with-animation :scroll-left="scrollLeft" @scroll="handleScroll">
      <view class="tab">
        <block v-for="(item,index) in data" :key="index">
          <view
            class="tab-item"
            :class="{'active':current === index}"
            :id="'scroll-tab-'+index"
            @tap="handleTabItemClick(index)"
          >{{item.title}}</view>
        </block>
        <view v-if="showPicker" class="picker-holder"></view>
      </view>
    </scroll-view>
    <picker
      v-if="showPicker"
      mode="selector"
      @change="bindTabChange"
      :value="current"
      :range="data"
      range-key="title"
    >
      <view class="picker"></view>
    </picker>
  </view>
</template>

<script>
export default {
  name: 'ScrollTabs',
  props: {
    data: {
      type: Array,
      default: () => []
    },
    active: {
      type: Number,
      default: 0
    },
    showPicker: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      scrollLeft: 0,
      current: 0
    };
  },
  watch: {
    active: function(val) {
      this.goToTab(val);
    }
  },
  methods: {
    handleTabItemClick(index) {
      this.goToTab(index);
    },
    goToTab(index) {
      if (this.current === index) {
        return;
      }
      this.current = index;
      const p1 = this.getLayout(".tab");
      const p2 = this.getLayout(`#scroll-tab-${index}`);
      Promise.all([p1, p2]).then(([res1, res2]) => {
        const outerLeft = res1.left;
        const outerWifth = res1.width;
        const innerLeft = res2.left;
        const innerWifth = res2.width;
        this.scrollLeft =
          innerLeft + innerWifth / 2 - (outerLeft + outerWifth / 2);
        this.$emit("tabChange", index);
      });
    },
    getLayout(selector) {
      return new Promise(resolve => {
        uni
          .createSelectorQuery()
          .in(this)
          .select(selector)
          .boundingClientRect(data => {
            resolve(data);
          })
          .exec();
      });
    },
    handleScroll() {
      this.moreVisiable = false;
    },
    bindTabChange(evt) {
      this.goToTab(evt.detail.value);
    }
  }
};
</script>

<style lang="scss" scoped>
$tab-item-height: 40px;

.container {
  height: $tab-item-height;
  position: relative;

  ::-webkit-scrollbar {
    display: none;
    width: 0 !important;
    height: 0 !important;
    -webkit-appearance: none;
    background: transparent;
  }
}

.tab {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  background: #fff;

  &-item {
    padding: 0 15px;
    flex: none;
    height: $tab-item-height;
    line-height: $tab-item-height;
    text-align: center;
    font-size: 14px;

    &.active {
      color: $uni-color-primary;
    }
  }
}

.picker-holder {
  height: $tab-item-height;
  width: $tab-item-height;
  flex: none;
}

.picker {
  position: absolute;
  top: 0;
  right: 0;
  height: $tab-item-height;
  width: $tab-item-height;
  background: #999;
}
</style>
