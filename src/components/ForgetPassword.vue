<template>
  <el-container class="mainContainer">
    <el-header>
      <el-steps :active="active" finish-status="success" align-center>
        <el-step title="用户信息确认"></el-step>
        <el-step title="密保验证"></el-step>
        <el-step title="输入新密码"></el-step>
      </el-steps>
    </el-header>
    <el-main>
     <ensure-user
        :curUserId="aaa.userId"
        @fillUserId="fillUserId"
        @next="next"
        @onsubmitOne="onsubmitOne"
        v-if="active == 0">
     </ensure-user>
     <safe-validate
        :safeQuestion="safeQuestion"
        :safeAnswer="safeAnswer"
        @next="next"
        v-if="active == 1">

     </safe-validate>
     <new-password
      :curUserId="aaa.userId"
      @next="next"
      v-if="active == 2">

     </new-password>
     <finish-page v-if="active == 3"></finish-page>
    </el-main>
  </el-container>
</template>

<script>
import EnsureUser from "./EnsureUser"
import SafeValidate from "./SafeValidate"
import NewPassword from "./NewPassword"
import FinishPage from './FinishPage';
export default {
  data () {
    return {
      active: 0,
      userId: '',
      safeQuestion:'',
      safeAnswer:'',
      aaa:{
        userId:""
      }
    }
  },
  methods: {
    next() {
      if (this.active++ > 2) this.active = 0;
    },
    fillUserId(userId){
      this.aaa.userId = userId
    },
    async onsubmitOne() {
        console.log("0")
        console.log(this.aaa.userId)
        //let aaa = {userId:this.userId}
        const {data:res} = await this.$http.post("getSafeInfo", this.aaa);//访问后台，await解析，赋值给res
        //去到了
        //res:{flag,safeQuestion,safeAnswer}
        if(res.flag == 'ok'){
          this.$message.success("操作成功！！！");
          console.log("0")
          console.log(res)
          this.safeQuestion = res.safeQuestion
          this.safeAnswer = res.safeAnswer
        }else{
          this.$message.error("操作失败！！！");
        }
      
    }
  },
  components: {
    EnsureUser,
    SafeValidate,
    NewPassword,
    FinishPage
  },
  computed: {
    showWhat() {
      if (this.active == 0){
        return "EnsureUser"
      }else if (this.active == 1){
        return "SafeValidate"
      }else if (this.active == 2){
        return "NewPassword"
      }
      else {
        return "FinishPage"
      }
    }
  }
}
</script>

<style>

</style>