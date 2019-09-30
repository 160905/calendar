<template>
  <div id="app">
    <MonthTabs @change="monthChange">
      <template #dateComp="{datestr}">
        <DateComp :dateStr="datestr">
          <template #dateItem="{data}">
            <div class="date">
              <span class="span">{{ data.date | timeFormat }}</span>
              <span></span>
            </div>
          </template>
        </DateComp>
      </template>
    </MonthTabs>
  </div>
</template>

<script>
import MonthTabs from "./components/MonthTabs.vue";
import DateComp from "./components/DateComp.vue";
export default {
  name: "app",
  components: {
    MonthTabs,
    DateComp
  },
  methods: {
    monthChange(date) {
      window.console.log(date);
    }
  },
  filters: {
    timeFormat: function(value, format = "DD日") {
      return value.format(format);
    }
  }
};
</script>
<style lang="stylus">
.in-month
  .date
    color #333333
.not-in-month
  .date
    color #999999
.today
  .date
    .span
      display inline-block
      text-align center
      border-radius 5px
      padding 5px
      font-size 12px
      background red
      color #ffffff
.weekend
  background #409EFF
</style>
