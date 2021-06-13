<template>
  <div class="mainContainer">
    <el-container class="tasks">
      <el-header height="30px">
        <span>进行中的任务：</span>
      </el-header>
      <el-main class="mainFrame">
        <el-carousel :interval="-1" type="card" height="650px">
          <el-carousel-item v-for="item in taskList" :key="item.id">
            <task-card 
              :taskId="item.taskId"
              :taskName="item.taskName"
              :description="item.description"
              :state="item.state"
              :publisherId="item.publisherId"
              :performerId="item.performerId"
              :timeLimit="item.timeLimit"
              :address="item.address"
              :reward="item.reward"
              >
            </task-card>
          </el-carousel-item>
        </el-carousel>
      </el-main>
      <el-footer class="adFrame" height="100px">
        <el-carousel :interval="5000" height="100px">
          <el-carousel-item v-for="item in 4" :key="item">
            <h3>广告demo{{ item }}</h3>
          </el-carousel-item>
        </el-carousel>
      </el-footer>
    </el-container>
    <el-container class="btns">
      <el-header height="30px" align='center'>
        <el-button 
        type="primary"
        @click="uploadTask()">
          发布任务<i class="el-icon-edit el-icon--right"></i>
        </el-button>
      </el-header>
    </el-container>
    <el-drawer
      title="新任务"
      :visible.sync="publishFlag"
      :direction="'btt'"
      :before-close="handleClose"
      :size="'60%'">
      <publish-task 
      @childrenCancle="childrenCancle"></publish-task>
    </el-drawer>
  </div>
</template>
  
<script>
import TaskCard from '../TaskCard'
import PublishTask from './PublishTask'
export default {
  data() {
    return {
      taskList: [
        // {
        // taskId: 10001,
        // taskName: "喂猫咪",
        // publisherId: 10001,
        // performerId: 10086,
        // description: "请帮忙喂养一下山东大学软件学院的猫咪，一天三次，一次50g猫粮",
        // state: 1,
        // timeLimit: 24,
        // address: "山东省济南市历下区舜华路1500号山东大学软件学院",
        // reward: "65535",
        // },
        // {
        // taskId: 10099,
        // taskName: "游戏代练",
        // publisherId: 10001,
        // // performerId: 10086,
        // description: "来个男生帮我打一下dnf的卢克团本,山东四区的剑帝和风法",
        // state: 0,
        // timeLimit: 8.5,
        // //address: "任何地方",
        // reward: "80",
        // },
        ],
      publishFlag:false
    }
  },
  methods: {
    uploadTask() {
      this.publishFlag = true
    },
    handleClose(done) {
      this.$confirm('确认关闭？')
        .then(_ => {
          done();
        })
        .catch(_ => {});
    },
    childrenCancle(){
      //无提示关闭抽屉
      this.publishFlag = false
    },
    
  },
  components: {
    TaskCard,
    PublishTask,

  },
  async mounted() {
    const { data: res } = await this.$http.post("getTaskByPublisher",JSON.parse(window.sessionStorage.getItem("user")).userId);
    console.log(res)
    this.taskList = res.tasks
    // this.taskList = res.tasks.filter((task) => {
      //   task.state != 3//不把已完成的任务显示出来
      // });
      //console.log(res.tasks)
  }
}
</script>

<style lang='less' scoped>
.mainContainer{
  justify-content: space-between;
}
.mainFrame{
  background-color: rgb(125, 199, 153);
}
.adFrame{
  background-color: rgb(226, 232, 236);
}
</style>