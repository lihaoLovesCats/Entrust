<template>
  <div class="login_container">
    <!-- 登录块-->
    <div class="login_box">
      <!-- 头像-->
      <div class="avatar_box">
      <img src="../assets/2.png" class="img-login" alt/>

      </div>
      <!--表单区域-->
      <el-form ref="loginFormRef" :rules="loginRules" :model="loginForm" class="login_form" label-width="0">
      <!--用户名-->
      <el-form-item prop="userId">
        <el-input v-model="loginForm.userId" prefix-icon="iconfont icon-denglu-copy1"></el-input>
      </el-form-item>
      
      <!-- 密码 -->
      <el-form-item prop="password">
        <el-input 
          ref="pwdField"
          v-model="loginForm.password" 
          prefix-icon="iconfont icon-denglumima" 
          type="password">
        </el-input>
      </el-form-item>
      <!-- 按钮 -->
      <el-form-item class="btns">
        <el-button type="primary" @click="login()">登录</el-button>
        <el-button type="info" @click="dialogVisible=true">忘记密码</el-button>
        <el-link class="label" type="primary" @click="registerFlag = true">还没有账户❓注册</el-link>
      </el-form-item>
      </el-form>
      <!-- 忘记密码部分 -->
      <el-dialog
        title="忘记密码"
        :visible.sync="dialogVisible"
        width="30%"
        :before-close="handleClose"
        append-to-body>
        <forget-password ></forget-password>
      </el-dialog>
    </div>
    <!-- 注册部分 -->
    <el-drawer
      title="注册"
      :visible.sync="registerFlag"
      :direction="'ltr'"
      :before-close="handleClose">
      <register class="regis"
        @childrenCancle="childrenCancle"
        @autoFill="autoFill">
      </register>
    </el-drawer>
  </div>
</template>
<script>
import Register from "./Register.vue";
import ForgetPassword from "./ForgetPassword.vue"

export default {
  data() {
    return {
      dialogVisible:false,
      //表单数据对象
      loginForm:{
      userId:"",
      password:""
      },
      //验证对象
      loginRules:{
      //校验用户名
      userId:[
        { required: true, message: '请输入用户名称', trigger: 'blur' },//关于必填项验证
        { min: 5, max: 12, message: '长度在 5 到 12 个字符', trigger: 'blur' }//长度验证
      ],
      //校验密码
      password:[
        { required: true, message: '请输入用户密码', trigger: 'blur' },//关于必填项验证
        { min: 6, max: 18, message: '长度在 6 ~ 18 个字符', trigger: 'blur' }//长度验证
      ],
      
      },
      registerFlag:false,
    }
  },
  methods: {
    //重置表单内容
    resetLoginForm(){
      this.$refs.loginFormRef.resetFields();
    },
    childrenCancle(){
      //无提示关闭抽屉
      this.registerFlag = false
    },
    autoFill(userId){
      //将注册用户名自动填入，然后将光标移到密码区域
      this.loginForm.userId = userId;
      this.loginForm.password = '';
      this.$refs.pwdField.focus()
      childrenCancle();
    },
    //登录方法
    login(){
      //验证校验规则
      
      this.$refs.loginFormRef.validate(async valid =>{
        if(!valid) return;
        const {data:res} = await this.$http.post("login", this.loginForm);//访问后台，await解析，赋值给res
        if(res.flag == 'ok'){
          this.$message.success("操作成功！！！");
          window.sessionStorage.setItem("user",JSON.stringify(res.user));
          //存储user对象,对象保存成str才能被储存
          this.$router.push({path:"/home"});//页面路由跳转
      
        }else{
          this.$message.error("操作失败！！！");
        }
      
      })

      //修改这里与后端交互👇
      // this.$alert(this.loginForm.userId+' '+this.loginForm.password,'登录成功',{
      // confirmButtonText: '确定',
      // callback: action => {
      //   this.$message({
      //     type: 'info',
      //     message: `action: ${action}`
      //   });
      //   window.sessionStorage.setItem("user",this.loginForm.userId);//存储user对象
      //   this.$router.push({path:"/home"});//页面路由跳转
      // }
      // });
      // this.$message.success("操作成功！！！");
      
    },
    handleClose(done) {
        this.$confirm('确认关闭？')
          .then(_ => {
            done();
          })
          .catch(_ => {});
    },
  },
  components: {
    Register,
    ForgetPassword,
  },
  


    Register}
</script>
<style lang="less" scoped>
//根节点样式
.login_container{
  background-color: #2b4b6b;
  height: 100%;
}
//
.login_box{
  width: 450px;
  height: 300px;
  background-color: #fff;
  border-radius: 3px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%);
  .avatar_box{
    width: 130px;
    height: 130px;
    border: 1px solid #eee;
    border-radius: 50%;
    padding: 5px;
    box-shadow: 0 0 2px #ddd;
    position: absolute;
    left: 50%;
    transform: translate(-50%,-50%);
    background-color: #eee;
    .img-login{
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background-color: #eee;
    }
  }
}
.btns{
  display: flex;
  justify-content: flex-end;
}
.label{
  position: absolute;
  left: -240px;
  
}
.login_form{
  position: absolute;
  bottom: 0%;
  width: 100%;
  padding: 0 10px;
  box-sizing: border-box;
}
</style>