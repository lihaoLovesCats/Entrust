<template>
  <div>
    <el-form ref="form" :rules="newPasswordRules" :model="form" label-width="80px">
      <el-form-item label="请输入您的新密码" prop="password">
        <el-input class="inputPart"
        v-model="form.password"
        clearable
        maxlength="18"
        show-word-limit
        type="password">
        </el-input>
      </el-form-item>
      <el-form-item label="确认密码" prop="rePassword">
        <el-input class="inputPart"
        v-model="form.rePassword"
        clearable
        maxlength="18"
        show-word-limit
        type="password">
        </el-input>
      </el-form-item>
    </el-form>
    
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
    var validatePassword = (rule,value,callback) => {
      //console.log(this.form.password)
      if (!value) {
        return callback(new Error('请再次确认密码！'))
      }
      else if (value != this.form.password) {
          callback(new Error('请确保两次输入的密码一致'))
      } else {
        callback()
      }
    }
    return{
      form: {
        password:'',
        userId: this.curUserId,
        rePassword:'',
      },
      
      newPasswordRules:{
        password:[
        { required: true, message: '请输入用户密码', trigger: 'blur' },//关于必填项验证
        { min: 6, max: 18, message: '长度在 6 ~ 18 个字符', trigger: 'blur' }//长度验证
        ],
        rePassword:[
        { required: true,trigger: 'blur',validator: validatePassword },//关于必填项验证
        { min: 6, max: 18, message: '请确保两次输入的密码一致', trigger: 'blur' }//长度验证
        ],
      },
    }
  },
  methods: {
    handleClick() {
      let obj = this.form
      this.$refs.form.validate(async valid =>{
        if(!valid) return;
        const {data:res} = await this.$http.post("forgetPassword", obj);//访问后台，await解析，赋值给res
        //res:{flag,userId}
        if(res == 'ok'){
          this.$message.success("操作成功！！！");
          this.$emit("next")
        }else{
          this.$message.error("操作失败！！！");
        }
      
      })
    }
  }
}
</script>

<style>

</style>