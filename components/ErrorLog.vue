<template>
  <Dialog v-model:visible="visible" title="未上报的错误日志" @close="$emit('update:visible', false)">
    <!-- <el-button type="primary" @click="() => {console.log(abc)}" v-if="true">生成</el-button> -->
    
    <div class="action-bar">
      <el-button :icon="Upload" @click="handleReport">上报</el-button>
      <el-button type="danger" :icon="Delete" @click="handleClear">清空</el-button>
      <el-button v-if="!isMute" type="warning" :icon="MuteNotification" @click="handleMute">静默捕获异常</el-button>
      <el-button v-else type="success" :icon="Bell" @click="handleDisableMute">启用异常提示</el-button>
    </div>

    <template v-if="logs.length">
      <el-timeline>
        <el-timeline-item
          v-for="logItem in logs"
          :timestamp="logItem.time"
          class="error-log-item"
          placement="top"
        >
          <el-card>
            <h3 class="message">{{logItem.message}}</h3>
            <p class="stack">{{logItem.stack}}</p>
          </el-card>
        </el-timeline-item>
      </el-timeline>
    </template>
    <template v-else>
      <el-empty description="没有错误日志" />
    </template>
  </Dialog>
</template>


<script setup lang="ts">
import { Upload, Delete, MuteNotification, Bell } from '@element-plus/icons-vue'

const visible = defineModel<boolean>('visible', { required: true })

const emit = defineEmits(['update:visible'])

const logs = ref<ErrorLogItem[]>([])
const isMute = ref(false)

interface ErrorLogItem {
  time: string
  message: string
  stack: string
}

onMounted(() => {
  getErrorLogs()
})

// 获取错误缓存列表
const getErrorLogs = () => {
  logs.value = JSON.parse(localStorage.getItem("errorNotReport") || "[]")
}

// 错误上报
const handleReport = () => {
  ElMessage({
    showClose: true,
    message: '开发中',
    type: 'warning',
  })
}

// 清空错误日志
const handleClear = () => {
  localStorage.setItem("errorNotReport", JSON.stringify([]))
  logs.value = []
  ElMessage({
    showClose: true,
    message: '日志已清空',
    type: 'success',
  })
}

// 静默异常捕获，不提示
const handleMute = () => {
  isMute.value = true
  ElMessage({
    showClose: true,
    message: '已启用静默',
    type: 'warning',
  })
  localStorage.setItem("isMuteErrorCatch", 'true')
}

// 启用异常捕获提示
const handleDisableMute = () => {
  isMute.value = false
  ElMessage({
    showClose: true,
    message: '已关闭静默',
    type: 'success',
  })
  localStorage.setItem("isMuteErrorCatch", 'false')
}
</script>

<style lang="scss" scoped>
ul {
  padding: 0;
}
.action-bar{
  margin-bottom: 10px;
  display: flex;
  justify-content: center;
}
.error-log-item {

  .message {
    margin: 0;
    color: #c45656;
  }
  .stack {
    white-space: pre-wrap;
  }
}
</style>