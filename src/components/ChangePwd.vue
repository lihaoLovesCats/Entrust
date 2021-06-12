<template>
    <el-form ref="form" :rules="changePwdRules" :model="form" label-width="80px">
      <el-form-item label="用户名" prop="userId">
        <el-input class="inputPart"
        v-model="form.userId"
        :disabled="true"
        clearable
        maxlength="18"
        show-word-limit>
        </el-input>
      </el-form-item>
      <el-form-item label="旧密码" prop="usedPassword">
        <el-input class="inputPart"
        v-model="form.usedPassword"
        clearable
        maxlength="18"
        show-word-limit
        type="password">
        </el-input>
      </el-form-item>
      <el-form-item label="新密码" prop="password">
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
      <el-form-item>
        <el-button type="primary" @click="onSubmit">确认修改</el-button>
        <el-button @click="handleClose()">取消</el-button>
      </el-form-item>
    </el-form>
</template>

<script>
export default {
  name: "ChangePwd",
  props: ['dialog'],
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
    
    return {
      form: {
        userId: '',
        usedPassword:'',
        password: '',
        rePassword: '',
      },
      changePwdRules:{
        usedPassword:[
        { required: true, message: '请输入用户密码', trigger: 'blur' },//关于必填项验证
        { min: 6, max: 18, message: '长度在 6 ~ 18 个字符', trigger: 'blur' }//长度验证
        ],
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
    onSubmit() {
      let obj = this.form
      this.$refs.form.validate(async valid =>{
        if(!valid) return;
        //这里用了put而不是post
        const {data:res} = await this.$http.post('changePassword',obj);//访问后台，await解析，赋值给res
        //console.log(res)
        if(res == 'ok'){
          this.$message.success("操作成功,请重新登录");
          this.$router.push({path:"/login"});//页面路由跳转
          window.sessionStorage.clear();//清空保存的用户数据
        }else{
          this.$message.error("操作失败！！！");
        }
      
      })
    },
    handleClose() {
      this.dialog.changeDialog.hide()
    }
  },
  mounted() {
    this.form.userId = JSON.parse(window.sessionStorage.getItem('user')).userId
  }
}
</script>

<style>

</style>