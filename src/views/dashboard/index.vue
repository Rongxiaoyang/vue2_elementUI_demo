<template>
  <div class="dashboard-container">
    <div class="dashboard-text">name: {{ name }}</div>
    <div class="block">
      <span class="demonstration">带快捷选项</span>
      <el-date-picker
        v-model="value"
        align="right"
        type="datetime"
        placeholder="选择日期"
        value-format="yyyy-MM-dd HH:mm:ss"
        :picker-options="{
          disabledDate(time) {
            return time.getTime() < Date.now()-86400000
          },
          selectableRange:selectableRange
        }"
      />
    </div>

  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import dayjs from 'dayjs'

export default {
  name: 'Dashboard',
  computed: {
    ...mapGetters([
      'name'
    ])

  },
  watch: {
    value: {

      handler(newValue, oldValue) {
        const dateTime = dayjs().format('YYYY-MM-DD HH:mm:ss') // 当前时间

        const dt = dateTime.split(' ')
        let minTime = ''
        // 是今天,选择 的时间开始为此刻的时分秒
        if (newValue.split(' ')[0] === dt[0]) {
          minTime = dt[1]
          if (!oldValue) {
            this.value = dateTime
          } else if (newValue.split(' ')[0] !== oldValue.split(' ')[0]) {
            this.value = dateTime
          }
        } else {
          minTime = '00:00:00'
        }
        this.selectableRange = minTime + ' - 23:59:59'
      }
    }
  },

  data() {
    return {
      selectableRange: '',
      value: ''
    }
  }
}
</script>

<style lang="scss" scoped>
.dashboard {
  &-container {
    margin: 30px;
  }
  &-text {
    font-size: 30px;
    line-height: 46px;
  }
}
</style>
