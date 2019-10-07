<template>
  <div id="app">
    <MonthTabs @input="monthChange" v-model="date" />
    <el-calendar :value="date" :first-day-of-week="1">
      <template #dateCell="{ date, data }">
        <div class="date">
          <span class="span">{{ data.day | timeFormat }}</span>
          <span class="belate" v-if="serviceData[data.day]">{{
            serviceData[data.day].belate
          }}</span>
        </div>
        <div v-if="serviceData[data.day]">
          <div v-if="serviceData[data.day].noData">
            {{ serviceData[data.day].noData }}
          </div>
          <div v-else-if="serviceData[data.day].leave">
            {{ serviceData[data.day].leave }}
          </div>
          <div
            v-else
            class="fields"
            v-for="(val, key) in serviceData[data.day].nomorl"
            :key="key"
          >
            <div class="field-item">
              <span>{{ key }}</span>
              <span>{{ val }}</span>
            </div>
          </div>
        </div>
        <div class="empty" v-else></div>
      </template>
    </el-calendar>
  </div>
</template>

<script>
import MonthTabs from "./components/MonthTabs.vue";
import dayjs from "dayjs";
export default {
  name: "app",
  data: function() {
    return {
      date: dayjs().format("YYYY-MM"),
      serviceData: {
        "2019-10-29": {
          nomorl: {},
          belate: "迟到",
          noData: ""
        },
        "2019-10-04": {
          nomorl: {},
          belate: "早退",
          noData: "无打卡记录",
          leave: ""
        },
        "2019-10-20": {
          nomorl: {
            早: "09:00:00",
            晚: "09:00:00"
          },
          belate: "",
          noData: "",
          leave: ""
        }
      }
    };
  },
  components: {
    MonthTabs
  },
  methods: {
    monthChange() {
      window.console.log(this.date);
    }
  },
  filters: {
    timeFormat: function(value, format = "DD日") {
      return dayjs(value).format(format);
    }
  }
};
</script>
<style lang="stylus">

.el-calendar
  font-size 14px
  .el-calendar__header
    display none
  .el-calendar__body
    .el-calendar-table
      th
        border-right 1px solid #cccccc
        border-top 1px solid #cccccc
        &:first-child
          border-left 1px solid #cccccc
      td
        border-color #cccccc !important
        .el-calendar-day
          padding 0
          height initial
          min-height 100px
.date
  height 38px
  display flex
  padding 0 8px
  align-items center
  border-bottom 1px solid #cccccc
  display flex
  justify-content space-between
.is-today
  .date
    .span
      display inline-block
      text-align center
      border-radius 5px
      padding 5px
      font-size 12px
      background red
      color #ffffff
.current
  .date
    color #333333
.pre,.next
  .date
    color #999999
.weekend
  background #409EFF
.fields
  padding-top 10px
  font-size 12px
  .field-item
    color #999999
.belate
  color #E6A23C
</style>
