<template>
  <ElDialog
    v-model="visible"
    @close="handleClose"
    @open="handleOpen"
    class="dialog animate__animated"
    :class="{ 'animate__zoomOut': disappearing, 'animate__zoomIn': !disappearing}"
    :width="width || '70%'"
    center
    :title="title"
  >
    <slot></slot>
  </ElDialog>
</template>

<script setup lang="ts">
const { title, width } = defineProps<{
  title: string,
  width?: string
}>()

const visible = defineModel<boolean>('visible', { required: true })

const disappearing = ref(false)

const handleClose = () => disappearing.value = true

// 打开弹窗时取消消失动画以防止弹窗打开后马上关闭
const handleOpen = () => disappearing.value = false
</script>

<style lang="scss">
// 弹窗动画持续时间修改为0.3秒
.animate__animated {
  &.animate__zoomIn, &.animate__zoomOut {
    --animate-duration: 0.3s;
  }
}
.dialog {
  min-width: 350px;
}
// 使element-plus对话框相对页面元素而非body
.el-overlay {
  position: absolute !important;
  .el-overlay-dialog {
    position: absolute !important;
  }
}
</style>