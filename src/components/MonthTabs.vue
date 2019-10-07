<template>
  <div class="box">
    <span class="icon pre" @click="preMonth">
      <i class="el-icon-arrow-left"></i>
    </span>
    <div class="tabs">
      <div
        :class="`tab ${item.name === value ? 'is-active' : ''}`"
        v-for="(item, index) in tabsArr"
        :key="index"
        @click="change(item.name, index)"
      >
        {{ item.label }}
      </div>
    </div>
    <span class="icon next" @click="nextMonth">
      <i class="el-icon-arrow-right"></i>
    </span>
  </div>
</template>
<script>
import dayjs from "dayjs";

export default {
  data: function() {
    return {
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
    },
    value: {
      type: String,
      default: dayjs().format("YYYY-MM"),
      required: true
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
      const preMonth = dayjs(this.value)
        .subtract(1, "month")
        .format("YYYY-MM");
      this.$emit("input", preMonth);
    },

    nextMonth() {
      this.index += 1;
      if (this.index - 6 === 1) {
        this.index = 6;
        this.tabCount = this.tabCount.map(num => num + 1);
      }
      const nextMonth = dayjs(this.value)
        .add(1, "month")
        .format("YYYY-MM");
      this.$emit("input", nextMonth);
    },

    change(selectMonth, index) {
      this.index = Number(index);
      this.$emit("input", selectMonth);
    }
  }
};
</script>
<style lang="stylus" scope>
.box
  height 45px
  display flex
  .icon
    background gray
    color #ffffff
    width 20px
    height 43px
    line-height 43px
    text-align center
    border 1px solid #DCDFE6
    border-top none
  .tabs
    flex 1
    display flex
    border-top 1px solid #cccccc
    background #e4e7ed
    .tab
      flex 1
      display flex
      border-right 1px solid #cccccc
      justify-content center
      border-bottom 1px solid #cccccc
      align-items center
      cursor pointer
      &:last-child
        border none
      &:hover
        color #409eff
    .is-active
      color #409eff
      border-bottom none
      background #ffffff
</style>
