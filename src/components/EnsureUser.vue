<template>
  <div>
    <el-input class="inputPart"
      v-model="userId"
      placeholder="其他用户将会看到的名字"
      clearable
      maxlength="10"
      show-word-limit>
    </el-input>
    <el-button
      type="primary"
      @click="handleClick">
      确认
    </el-button>
  </div>
</template>

<script>
export default {
  props: ["curUserId"],
  data() {
    return {
      userId: this.curUserId
    }
  },
  methods: {
    handleClick() {
      this.$emit("fillUserId",this.userId)
      this.$emit("onsubmitOne")
      this.$emit("next")
    }
  }
}
</script>

<style>

</style>