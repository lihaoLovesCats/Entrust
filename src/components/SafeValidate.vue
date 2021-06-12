<template>
  <div>
    <el-form>
      <el-form-item label="密保问题">
        <el-input class="inputPart"
          v-model="Questi"
          :disabled="true">
        </el-input>
      </el-form-item>
      <el-form-item label="密保答案">
          <el-input class="inputPart"
          v-model="curSafeAnswer"
          clearable
          maxlength="10"
          show-word-limit>
          </el-input>
        </el-form-item>
    </el-form>
    
    <el-button
      type="primary"
      @click="handleClick"
      style="float: right">
      确认
    </el-button>
  </div>
</template>

<script>
export default {
  props: ["safeQuestion","safeAnswer"],
  data () {
    return {
      curSafeAnswer:'',
      //aaa:this.safeQuestion
    }
  },
  methods: {
    handleClick() {
      if(this.curSafeAnswer == this.safeAnswer){
        //this.$emit("fillUserId(this.userId)")
        this.$emit("next")
      }
      else{
        this.$message.error("密保问题不正确,操作失败");
      }
    }
  },
  computed: {
    Questi () {
      if (this.safeQuestion == 0) {
        return "我的父亲的名字是？"
      }else if (this.safeQuestion == 1){
        return "我的母亲的名字是？"
      }
      else if (this.safeQuestion == 2){
        return "我的宠物的名字是？"
      }
      else if (this.safeQuestion == 3){
        return "我的高中老师的名字是？"
      }
      else if (this.safeQuestion == 4){
        return "我的初中老师的名字是？"
      }
      else if (this.safeQuestion == 5){
        return "我的小学老师的名字是？"
      }else {
        return "我的配偶的名字是？"
      }
    }
  }
}
</script>

<style>

</style>