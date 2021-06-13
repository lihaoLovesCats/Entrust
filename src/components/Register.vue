<template>
  <div>
    <el-form ref="form" :rules="registerRules" :model="form" label-width="80px">
      <el-form-item label="昵称">
        <el-input class="inputPart"
        v-model="form.nickname"
        placeholder="其他用户将会看到的名字"
        clearable
        maxlength="10"
        show-word-limit></el-input>
      </el-form-item>
      <el-form-item label="角色">
        <el-select v-model="form.role">
          <el-option label="执行者" value="0"></el-option>
          <el-option label="发布者" value="1"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="电话号码" prop="phone">
        <el-input class="inputPart"
        v-model="form.phone"
        placeholder="不可修改"
        clearable
        maxlength="11"
        show-word-limit>
        </el-input>
      </el-form-item>
      <el-form-item label="密码" prop="password">
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
      <el-form-item label="性别">
        <el-select v-model="form.sex">
          <el-option label="女" value="0"></el-option>
          <el-option label="男" value="1"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="生日">
        <el-col :span="11">
          <el-date-picker type="date" 
          placeholder="选择日期" 
          v-model="form.birthday" 
          style="width: 100%;">
          </el-date-picker>
        </el-col>
      </el-form-item>
      <el-form-item label="个性签名">
        <el-input
          class="inputPart"
          type="textarea"
          placeholder="描述一下自己吧！"
          v-model="form.motto"
          maxlength="40"
          show-word-limit
        >
        </el-input>
      </el-form-item>
      <el-form-item label="密保问题">
        <el-select v-model="form.safeQuestion">
          <el-option label="我的父亲的名字是？" value="0"></el-option>
          <el-option label="我的母亲的名字是？" value="1"></el-option>
          <el-option label="我的宠物的名字是？" value="2"></el-option>
          <el-option label="我的高中老师的名字是？" value="3"></el-option>
          <el-option label="我的初中老师的名字是？" value="4"></el-option>
          <el-option label="我的小学老师的名字是？" value="5"></el-option>
          <el-option label="我的配偶的名字是？" value="6"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="密保答案">
        <el-input class="inputPart"
        v-model="form.safeAnswer"
        clearable
        maxlength="10"
        show-word-limit>
        </el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">立即创建</el-button>
        <el-button @click="handleCancle">取消</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
export default {
  name: "Register",
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
        phone: '',
        nickname: '',
        role:'',
        sex: '',
        password: '',
        rePassword: '',
        birthday: '',
        motto:'',
        safeQuestion:'',
        safeAnswer:'',
        credit:10000
      },
      registerRules:{
        phone:[
        { required: true, message: '请输入电话号码', trigger: 'blur' },//关于必填项验证
        { min: 11, max: 11, message: '请输入正确的电话号码', trigger: 'blur' }//长度验证
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
      console.log(obj)
      this.$refs.form.validate(async valid =>{
        if(!valid) return;
        const {data:res} = await this.$http.post("addUserr", obj);//访问后台，await解析，赋值给res
        //res:{flag,userId}
        if(res.flag == 'ok'){
          this.$message.success("操作成功！！！");
          this.$alert("请牢记，您的id为："+res.userId,{confirmButtonText: '确定'})
          //this.$emit('autoFill',res.userId)
          this.$emit("childrenCancle")
        }else{
          this.$message.error("操作失败！！！");
        }

      })
    },
    handleCancle(){
      this.$emit("childrenCancle")
    }
  },
}
</script>

<style lang='less' scoped>
.inputPart{
  width: 90%;
}
</style>