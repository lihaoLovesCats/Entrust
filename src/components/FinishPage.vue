<template>
  <div>
    <h2>恭喜您已成功完成本次业务，页面将在3秒内跳转...</h2>
     <el-button
      type="primary"
      @click="handleClick">
      未成功跳转❓请手工跳转
    </el-button>
  </div>
</template>

<script>
export default {
  methods: {
    handleClick(){
      console.log('1')
      this.$router.go(0);//页面路由跳转
    }
  }
}
</script>

<style>

</style>