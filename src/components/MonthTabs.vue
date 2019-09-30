<template>
  <div class="box">
    <span class="icon pre" @click="preMonth">
      <i class="el-icon-arrow-left"></i>
    </span>
    <span class="icon next" @click="nextMonth">
      <i class="el-icon-arrow-right"></i>
    </span>
    <el-tabs v-model="activeDate" type="border-card" @tab-click="handleClick">
      <el-tab-pane
        v-for="(item, index) in tabsArr"
        :key="index"
        :label="item.label"
        :name="item.name"
      />
    </el-tabs>
    <main class="main">
      <slot name="dateComp" :datestr="activeDate"></slot>
    </main>
  </div>
</template>
<script>
import dayjs from "dayjs";

export default {
  data: function() {
    return {
      activeDate: "2019-09",
      dateObj: dayjs(),
      tabCount: [0, 1, 2, 3, 4, 5, 6],
      index: 0
    };
  },
  props: {
    formatTab: {
      type: String,
      default: "YYYY年MM月",
      required: false
    }
  },
  computed: {
    tabsArr: function() {
      return this.tabCount.map(num => {
        return {
          label: this.dateObj.add(num, "month").format(this.formatTab),
          name: this.dateObj.add(num, "month").format("YYYY-MM")
        };
      });
    }
  },
  methods: {
    preMonth() {
      this.index -= 1;
      if (this.index - 0 === -1) {
        this.tabCount = this.tabCount.map(num => num - 1);
        this.index = 0;
      }
      this.activeDate = dayjs(this.activeDate)
        .subtract(1, "month")
        .format("YYYY-MM");
      this.$emit("change", this.activeDate);
    },

    nextMonth() {
      this.index += 1;
      if (this.index - 6 === 1) {
        this.index = 6;
        this.tabCount = this.tabCount.map(num => num + 1);
      }
      this.activeDate = dayjs(this.activeDate)
        .add(1, "month")
        .format("YYYY-MM");
      this.$emit("change", this.activeDate);
    },

    handleClick(tab) {
      this.index = Number(tab.index);
      this.$emit("change", this.activeDate);
    }
  }
};
</script>
<style lang="stylus" scope>
.box
  position relative
  .icon
    position absolute
    background gray
    color #ffffff
    width 20px
    height 40px
    line-height 40px
    text-align center
    z-index 3
  .pre
    left 0
  .next
    right 0

  .main
    padding 0 15px 15px
    background #ffffff
    border 1px solid #DCDFE6
    border-top none

  .el-tabs--border-card
    border-bottom none
    box-shadow none
  .el-tabs__nav
    width 100%
    padding 0 10px 0 15px
  .el-tabs__item
    width calc(100% / 7)
    text-align center
    border-right 1px solid #cccccc !important
</style>
