<template>
  <Dialog
    v-model:visible="visible"
    title="道具使用"
    height="400px"
  >
    <div class="item-dialog-box">
      <div class="owned-count"> 当前拥有： {{ count }} 个</div>
      <div class="description"> {{ itemData.description }} </div>
      <ElButton @click="getItem">获得一个</ElButton>
      <ElButton @click="useItem">使用一个</ElButton>
    </div>
  </Dialog>
</template>

<script setup lang="ts">
import { ElMessage, ElNotification } from 'element-plus'
import { useGameStore } from '~/store'

const gameStore = useGameStore()

const visible = defineModel<boolean>('visible', { required: true })
const props = defineProps<{
  itemData: {
    prop: 'refresh' | 'back'
    description: string
  }
}>()

const count = ref(0)

const getItem = () => {
  count.value++
  ElNotification({
    title: '获得道具',
    message: `获得一个${props.itemData.prop}道具`
  })
}

const useItem = () => {
  if (count.value == 0) {
    ElNotification({
      title: '道具使用失败',
      message: '道具剩余数量不够啦',
      duration: 2000,
      type: 'warning'
    })
    return null
  }

  // 道具名与其功能方法映射
  const operation = {
    'refresh': gameStore.refreshDeck,
    'back': gameStore.moveAllToDeck
  }
  // 扣除道具数量
  count.value--
  // 执行道具对应功能
  operation[props.itemData.prop]()
  ElMessage({
    message: '道具使用成功',
    type: 'success'
  })
  // 关闭道具对话框
  visible.value = false
}
</script>

<style lang="scss" scoped>
.item-dialog-box {
  display: flex;
  flex-direction: column;
  align-items: center;

  .el-button+.el-button{
    margin: 0;
  }

  >* {
    margin-bottom: 10px;
  }
}
</style>
