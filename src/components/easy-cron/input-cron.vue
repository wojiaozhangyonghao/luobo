<template>
  <div class="input-cron">
    <el-input :placeholder="placeholder" style="" v-model="editCronValue" :disabled="disabled">
      <a slot="append" @click="showConfigDlg" class="config-btn" :disabled="disabled">
        <i class="el-icon-s-tools" style="margin-right: 5px;"></i>配置
      </a>
    </el-input>
    <el-dialog :visible.sync="show" title="配置Cron表达式" :closable="true" :width="`${width+50}`"
      :footer-hide="true">
        <div>
          <easy-cron v-model="editCronValue" :style="`width: ${width}px`" :exeStartTime="exeStartTime"
           ></easy-cron>
        </div>
    </el-dialog>
     <!-- :hideYear="hideYear" :remote="remote" :hideSecond="hideSecond" -->
  </div>
</template>

<script>
import EasyCron from './index'

export default {
  name: 'input-cron',
  model: {
    prop: 'cronValue',
    event: 'change'
  },
  props: {
    cronValue: {
      type: String,
      default: ''
    },
    width: {
      type: Number,
      default: 750
    },
    placeholder: {
      type: String,
      default: '请输入cron表达式'
    },
    disabled: {
      type: Boolean,
      default: false
    },
    exeStartTime: {
      type: [Number, String, Object],
      default: 0
    },
    hideSecond: {
      type: Boolean,
      default: false
    },
    hideYear: {
      type: Boolean,
      default: false
    },
    remote: {
      type: Function,
      default: null
    }
  },
  data () {
    return {
      editCronValue: this.cronValue,
      show: false
    }
  },
  watch: {
    cronValue (newVal, oldVal) {
      if (newVal === this.editCronValue) {
        return
      }
      this.editCronValue = newVal
    },
    editCronValue (newVal, oldVal) {
      this.$emit('change', newVal)
    }
  },
  methods: {
    showConfigDlg () {
      if (!this.disabled) {
        this.show = true
      }
    }
  },
  components: {
    EasyCron
  }
}
</script>

<style scoped>
.input-cron, .input-ui {
  /* // display: inline-block; */
}

.input-cron .ivu-input-wrapper {
  width: 100% !important;
}

.config-btn {
  cursor: pointer;
}

.config-btn:hover {
  color: #2D8CF0;
}
</style>
