<template>
  <el-card class="box-card">
    <div slot="header" class="clearfix">
      <span>任务{{taskId}}号：{{taskName}}</span>
      <el-button 
        v-if="showBtn&&receivable"
        style="float: right; padding: 5px " 
        type="button"
        @click="receiveTask"
        >
        接受任务
      </el-button>
      <el-button
          v-if="!showBtn&&state!=2&&!receivable"
          style="float: right; padding: 5px "
          type="button"
          @click="stopTaskByPublisher"
          
      >
        取消任务
      </el-button>
      <el-button
          v-if="state==2"
          style="float: right; padding: 5px "
          type="button"
          @click="finishTask"
      >
        完成任务
      </el-button>
      <!-- <el-button style="float: right; padding: 3px 0" type="text">查看详情</el-button> -->
    </div>
    <div>
      <el-divider content-position="left">任务描述</el-divider>
      <span>{{description}}</span>
      <el-divider content-position="left">发布者</el-divider>
      <span>{{publisherId}}</span>
      <el-divider content-position="left">任务状态</el-divider>
      <span>{{viewState}}</span>
      <template v-if="state!==0">
        <el-divider content-position="left">执行者</el-divider>
        <span>{{performerId}}</span>
      </template>
       <template v-if="timeLimit">
        <el-divider content-position="left">执行时间</el-divider>
        <span>{{taskTime}}</span>
      </template>
      <template v-if="address">
        <el-divider content-position="left">任务地点</el-divider>
        <span>{{address}}</span>
      </template>
      <template v-if="reward">
        <el-divider content-position="left">任务佣金</el-divider>
        <span>BTC:{{reward}}</span>
      </template>
    </div>
  </el-card>
</template>

<script>
export default {
  props: [
    'taskId',
    'publisherId',
    'publisherName',
    'performerId',
    'performerName',
    'taskName',
    'description',
    'state',
    'timeLimit',
    'address',
    'reward',
    'showBtn',
    'receivable'
  ],
  data () {
    return {
    }
  },
  computed: {
    viewState() {
      if (this.state===0) {
        return "未接收"
      }else if (this.state===1) {
        return "任务执行中"
      }
      else if (this.state===2) {
        return "执行者汇报完成"
      }
      else if (this.state===3) {
        return "发布者确认完成"
      }
      else {
        return "任务异常"
      }
    },
    taskTime() {
      return new Date(this.timeLimit).toLocaleDateString()
    }
  },
  methods: {
    async receiveTask() {
      let obj = {
        performerId: JSON.parse(window.sessionStorage.getItem("user")).userId,
        taskId: this.taskId
      }
      const {data:res} = await this.$http.post("receiveTask", obj);//访问后台，await解析，赋值给res
      this.$router.go(0);//刷新当前页面
    },
    async stopTaskByPublisher() {
      const { data:res } = await this.$http.post("stopTaskByPublisher",this.taskId);
      this.$router.go(0);//刷新当前页面
    },
    async finishTask() {
      const { data:res } = await this.$http.post("finishTaskPublisher",this.taskId);
      this.$router.go(0);//刷新当前页面
    }
  }
}
</script>

<style lang='less' scoped>
  .box-card{
    height: 700px;
    width: 500px;
  }


</style>