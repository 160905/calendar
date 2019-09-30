<template>
  <div>
    <div class="week-box">
      <div class="item" v-for="name in weekName" :key="name">{{ name }}</div>
    </div>
    <div class="day-box">
      <div
        @click="dayClick(item.date)"
        :class="`item ${item.className}`"
        v-for="(item, i) in days"
        :key="i"
      >
        <slot name="dateItem" :data="item"></slot>
      </div>
    </div>
  </div>
</template>
<script>
import dayjs from "dayjs";

export default {
  data: function() {
    return {
      weekName: ["周一", "周二", "周三", "周四", "周五", "周六", "周日"]
    };
  },
  props: {
    dateStr: { type: String, default: "2019-09", required: true }
  },
  computed: {
    days: function() {
      const arr = [];
      const firstDayCount = this.everyMonthFirstDayCount();
      for (let i = 0; i < 42; i++) {
        const date = this.getDayDate(i - firstDayCount + 1);
        const className = this.getClassName(i - firstDayCount, date);
        arr.push({
          date,
          className
        });
      }
      return arr;
    },
    everyMonthMaxDay: function() {
      return dayjs(this.dateStr).daysInMonth();
    }
  },
  methods: {
    dayClick(date) {
      this.$emit("dayClick", date.format("YYYY-MM-DD"));
    },
    getClassName(dt, date) {
      let className = "not-in-month";
      if (dt > -1 && dt < this.everyMonthMaxDay) {
        className = "in-month";
        if (date.format("YYYY-MM-DD") === dayjs().format("YYYY-MM-DD")) {
          className = "today";
        }
      }
      return className;
    },
    getDayDate(dt) {
      let monthToday = dayjs(`${this.dateStr}-01`);
      if (dt < 0) {
        monthToday = monthToday.subtract(Math.abs(dt), "day");
      } else {
        monthToday = monthToday.add(Math.abs(dt), "day");
      }
      return monthToday;
    },
    everyMonthFirstDayCount() {
      return dayjs(`${this.dateStr}-01`).day();
    }
  }
};
</script>

<style lang="stylus" scope>
.week-box
  overflow hidden
  border 1px solid #cccccc
  .item
    float left
    width calc(100% / 7)
    border-right 1px solid #cccccc
    text-align center
    height 50px
    line-height 50px
    &:last-child
      border none
.day-box
  border 1px solid #cccccc
  border-bottom none
  border-top none
  overflow hidden
  .item
    float left
    width calc(100% / 7)
    border-right 1px solid #cccccc
    border-bottom 1px solid #cccccc
    height 120px
    display flex
    flex-direction column
    .date
      height 38px
      padding 0 8px
      display flex
      align-items center
      border-bottom 1px solid #cccccc
      display flex
      justify-content space-between
    &:nth-child(7n)
      border-right none
    .other
      flex 1
      display flex
      justify-content center
      align-items center
      &:hover
        background green
</style>
