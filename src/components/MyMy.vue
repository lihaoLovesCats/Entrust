<template>
  <div>
    <el-divider content-position="left">通常</el-divider>
    <el-form ref="form" :rules="registerRules" :model="form" label-width="80px">
      <el-form-item label="用户名" >
        <el-input class="inputPart"
        v-model="form.userId"
        placeholder="作为标识用户的唯一id,不可修改"
        :disabled="true"
        maxlength="12"
        show-word-limit>
        </el-input>
      </el-form-item>
      <el-form-item label="角色">
        <el-select v-model="form.role" :disabled="true">
          <el-option label="执行者" value="0"></el-option>
          <el-option label="发布者" value="1"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="昵称" prop="userId">
        <el-input class="inputPart"
        v-model="form.nickname"
        placeholder="其他用户将会看到的名字"
        clearable
        maxlength="10"
        show-word-limit></el-input>
      </el-form-item>
      <el-form-item label="性别">
        <el-select v-model="form.userSex">
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
      <el-form-item>
        <el-button type="primary" @click="onSubmit">确认修改</el-button>
        <el-button @click="handleClose()">取消</el-button>
      </el-form-item>
    </el-form>
    <el-divider content-position="left">安全</el-divider>
    <el-button class="changepwdbtn" type="danger" plain @click="dialogTableVisible=true">修改密码</el-button>
    <el-dialog 
    ref="changeDialog"
    title="修改密码" 
    :visible.sync="dialogTableVisible"
    append-to-body>
      <change-pwd :dialog='ObjDialog'></change-pwd>
    </el-dialog>
  </div>
</template>

<script>
import ChangePwd from './ChangePwd'
export default {
  name: "MyMy",
  props: ['drawer'],
  data() {
    return {
      ObjDialog: this.$refs,
      form: {
        nickname: '',
        userId: '',
        role:'',
        userSex: '',
        birthday: '',
        motto:'',
      },
      registerRules:{
       
      },
      dialogTableVisible: false,
    }
  },
  methods: {
    changeAvatar() {
      this.$message.error("更换头像功能还未实现！");
    },
    handleClose() {
      this.drawer.drawerMy.hide()
    },
    onSubmit() {
      let obj = this.form
      this.$refs.form.validate(async valid =>{
        if(!valid) return;
        //请求后端重新发送一个user
        const {data:res} = await this.$http.post('changeInfo',obj);//访问后台，await解析，赋值给res
        if(res.flag == 'ok'){
          this.$message.success("操作成功!!!");
          this.handleClose()
          window.sessionStorage.clear();//清空保存的用户数据
          window.sessionStorage.setItem(JSON.stringify(res.user))
        }else{
          this.$message.error("操作失败！！！");
        }
      
      })
    },
  },
  computed: {
    sexx() {
      return form.userSex==0? "女":"男"
    }
  },
  components: {
    ChangePwd
  },
  mounted(){
    //用户信息初始化
    let user = JSON.parse(window.sessionStorage.getItem("user"))
    this.form.nickname = user.nickname
    this.form.userId = user.userId
    this.form.role = user.role+""
    this.form.userSex = user.sex+""
    this.form.birthday = user.birthday
    this.form.motto = user.motto
  },
}
</script>

<style lang='less' scoped>
.inputPart{
  width: 90%;
}
.changepwdbtn{
  display: flex;
  margin-left: 30%;
  
}
</style>