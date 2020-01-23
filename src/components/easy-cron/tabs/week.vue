<template>
  <div class="config-list">
    <el-radio-group v-model="type">
    <div class="item">
      <el-radio label="TYPE_NOT_SET" class="choice" :disabled="disableChoice">不设置</el-radio>
      <span class="tip-info">日和周只能设置其中之一</span>
    </div>
    <div class="item">
      <el-radio label="TYPE_RANGE" class="choice" :disabled="disableChoice">区间</el-radio>
       从<el-select v-model="valueRange.start"  class="w80" :disabled="type!=TYPE_RANGE || disableChoice">
        <el-option v-for="v in WEEK_MAP" :value="v.value" :label="v.label"  :key="v.value"></el-option>
      </el-select>
       至<el-select v-model="valueRange.end"  class="w80" :disabled="type!=TYPE_RANGE || disableChoice">
        <el-option v-for="v in WEEK_MAP" :value="v.value" :label="v.label" :key="v.value"></el-option>
      </el-select>
    </div>
    <div class="item">
      <el-radio label="TYPE_LOOP" class="choice" :disabled="disableChoice">循环</el-radio>
      从<el-select v-model="valueLoop.start"  class="w80" :disabled="type!=TYPE_LOOP || disableChoice">
        <el-option v-for="v in WEEK_MAP" :value="v.value" :label="v.label" :key="v.value"></el-option>
      </el-select>开始，间隔
      <el-input-number size="mini" :disabled="type!=TYPE_LOOP || disableChoice" :max="maxValue" :min="minValue" :precision="0"
       class="w60" v-model="valueLoop.interval" /> 天
    </div>
    <div class="item">
      <el-radio  label="TYPE_SPECIFY" class="choice" :disabled="disableChoice">指定</el-radio>
      <div class="list">
        <el-checkbox-group v-model="valueList">
          <el-checkbox class="list-check-item" v-for="v in WEEK_MAP"
            :label="v.value" :key="v.value" :disabled="type!=TYPE_SPECIFY || disableChoice">{{v.label}}</el-checkbox>
        </el-checkbox-group>
      </div>
    </div>
    </el-radio-group>
  </div>
</template>

<script>
import mixin from './mixin'
import { WEEK_MAP_EN, replaceWeekName } from './const.js'

// const WEEK_MAP = {
//   '周日': 0,
//   '周一': 1,
//   '周二': 2,
//   '周三': 3,
//   '周四': 4,
//   '周五': 5,
//   '周六': 6
// }
const WEEK_MAP = [
  {label:'周日',value:0},
  {label:'周一',value:1},
  {label:'周二',value:2},
  {label:'周三',value:3},
  {label:'周四',value:4},
  {label:'周五',value:5},
  {label:'周六',value:6}
  ]
export default {
  name: 'week',
  mixins: [mixin],
  props: {
    day: {
      type: String,
      default: '*'
    }
  },
  data () {
    return {
      WEEK_MAP,
      WEEK_MAP_EN
    }
  },
  computed: {
    disableChoice () {
      return (this.day && this.day !== '?') || this.disabled
    }
  },
  watch: {
    value_c (newVal, oldVal) {
      // 如果设置日，那么星期就直接不设置
      this.updateValue()
    },
    day (newVal) {
      // console.info('new day: ' + newVal)
      this.updateValue()
    }
  },
  methods: {
    updateValue () {
      this.$emit('change', this.disableChoice ? '?' : this.value_c)
    },
    preProcessProp (c) {
      return replaceWeekName(c)
    }
  },
  created () {
    this.DEFAULT_VALUE = '*'
    // 0,7表示周日 1表示周一
    this.minValue = 0
    this.maxValue = 6
    this.valueRange.start = 0
    this.valueRange.end = 6
    this.valueLoop.start = 2
    this.valueLoop.interval = 1
    this.parseProp(this.prop)
  }
}
</script>

<style scoped>

.el-radio-group {
  font-size: 14px;
}
.el-radio{
  margin-right:0;
}
.el-checkbox {
  margin-right:0;
}
.config-list {
  text-align: left;
  margin: 0 10px 10px 0px;
}

.item {
  margin-top: 5px;
}

.tip-info {
  color: #999
}

.choice {
  border: 1px solid transparent;
  padding: 5px 8px;
}

.choice:hover {
  border: 1px solid #2d8cf0;
}

.w80 {
  width: 100px;
}
.w60{
  width:80px;
}

.ivu-input-number, .ivu-select {
  margin-left: 5px;
  margin-right: 5px;
}

.list {
  /* margin: 0 20px; */
}

.list-check-item {
  padding: 1px 3px;
  width: 4em;
}
</style>
