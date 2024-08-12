<template>
  <div class="home h-full w-full flex flex-col justify-center items-center">
    <img class="w[60%] mb-24" src="/img/title.png">

    <NuxtLink to="/game" class="start-game home-button">开始游戏</NuxtLink>
    <div class="change-log home-button" @click="openChangeLog">更新日志</div>
    <div class="error-log home-button" @click="openErrorLog">错误日志</div>

    <ChangeLog v-model:visible="changeLogVisible"></ChangeLog>
    <ErrorLog v-model:visible="errorLogVisible"></ErrorLog>

    <div class="copyright">
      Developed by <el-link href="https://github.com/Ximu-Luya" target="_blank">@Ximu-Luya</el-link> 
      Ver {{version}}
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue"
import changelogdata from "~/assets/changelog.json"

const version = ref(changelogdata[0].version)
const changeLogVisible = ref(false)
const errorLogVisible = ref(false)

const openChangeLog = () => {
  changeLogVisible.value = true
}

const openErrorLog = () => {
  errorLogVisible.value = true
}

</script>

<style lang="scss" scoped>
.home {
  .home-button {
    position: relative;
    padding: 15px 20px;
    width: 35%;
    max-width: 360px;
    line-height: 24px;
    text-align: center;
    border-radius: 5px;
    color: #fff;
    font-size: 20px;
    margin-bottom: 20px;
    font-weight: bold;

    &:active{
      top: 8px;
      box-shadow: 0 1px 6px rgb(0 0 0 / 20%) inset;
    }
  }
  .start-game {
    border: 1px solid #9bcf60;
    box-shadow: 0 0 0 1px #469a44, 1px 5px 0 #469a44, -1px 5px 0 #469a44, 0 6px 8px rgb(0 0 0 / 80%);
    background: -webkit-linear-gradient(top,#8dd845,#6fbb31);
    &:active {
      background: -webkit-linear-gradient(top,#6fbb31,#6fbb31);
    }
  }
  .change-log {
    border: 1px solid #e5bf76;
    box-shadow: 0 0 0 1px #88683b, 1px 5px 0 #88683b, -1px 5px 0 #88683b, 0 6px 8px rgb(0 0 0 / 80%);
    background: -webkit-linear-gradient(top,#e0b956,#bb8231);
    &:active {
      background: -webkit-linear-gradient(top,#bb8231,#bb8231);
    }
  }
  .error-log {
    border: 1px solid #ef8585;
    box-shadow: 0 0 0 1px #c45656, 1px 5px 0 #c45656, -1px 5px 0 #c45656, 0 6px 8px rgb(0 0 0 / 80%);
    background: -webkit-linear-gradient(top,#ef8585, #F56C6C);
    &:active {
      background: -webkit-linear-gradient(top,#F56C6C,#F56C6C);
    }
  }

  .copyright {
    position: absolute;
    right: 0; bottom: 0;
    padding: 20px;
    display: flex;
    align-items: center;

    >* {
      padding: 0 5px;
    }
  }
}
</style>