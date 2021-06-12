<template>
  <div>
    <el-form ref="form" :rules="publishRules" :model="form" label-width="80px">
      <el-form-item label="任务名称" prop="taskName">
        <el-input class="inputPart"
        v-model="form.taskName"
        placeholder="帮助执行者快速了解任务内容"
        clearable
        maxlength="10"
        show-word-limit></el-input>
      </el-form-item>
      <el-form-item label="任务描述" prop="description">
        <el-input class="inputPart"
        v-model="form.description"
        type="textarea"
        placeholder="请详细描述任务细节"
        clearable
        maxlength="80"
        show-word-limit>
        </el-input>
      </el-form-item>
      <el-form-item label="执行日期" prop="timeLimit">
        <el-col :span="11">
          <el-date-picker type="date" 
          placeholder="选择日期" 
          v-model="form.timeLimit" 
          style="width: 100%;"
          :picker-options="pickerOptions">
          </el-date-picker>
        </el-col>
      </el-form-item>
      <el-form-item label="任务地点" prop="address">
        <el-input
          class="inputPart"
          type="textarea"
          placeholder="请明确任务地点"
          v-model="form.address"
          maxlength="40"
          show-word-limit
        >
        </el-input>
      </el-form-item>
      <el-form-item label="佣金" prop="reward">
        <el-input class="inputPart"
        v-model="form.reward"
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
    return {
      form: {
        taskName: '',
        description: '',
        timeLimit:'',
        address: '',
        reward: '',
        publisherId: JSON.parse(window.sessionStorage.getItem("user")).userId
      },
      pickerOptions:{
        disabledDate(time) {
            return time.getTime() < Date.now();
        },
      },
      publishRules:{
        taskName:[
        { required: true, message: '请输入任务名称', trigger: 'blur' },//关于必填项验证
        ],
        timeLimit:[
        { required: true, message: '请选择日期', trigger: 'blur' },//关于必填项验证
        ],
        description:[
        { required: true, message: '请详细描述任务细节', trigger: 'blur' },//关于必填项验证
        ],
        address:[
        { required: true, message: '请明确任务地点', trigger: 'blur' },//关于必填项验证
        ],
        reward:[
        { required: true, message: '请输入任务佣金', trigger: 'blur' },//关于必填项验证
        ],
      },
    }
  },
  methods: {
    onSubmit() {
      let obj = this.form
      console.log(this.form.publisherId)
      this.$refs.form.validate(async valid =>{
        if(!valid) return;
        const {data:res} = await this.$http.post("publishTask", obj);//访问后台，await解析，赋值给res
        //res:{flag,userId}
        if(res == 'ok'){
          this.$message.success("操作成功！！！");
          this.$router.go(0);//刷新当前页面
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

<style>
  .inputPart{
    width: 50%;
  }
</style>