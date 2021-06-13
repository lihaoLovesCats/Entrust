<template>
  <div class="mainContainer">
    <div class="tasklist">
      <el-container class="tasks">
        <el-header height="30px">
          <span>任务清单：</span>
        </el-header>
        <el-main class="mainFrame">
          <el-carousel :interval="-1" type="card" height="800px">
            <el-carousel-item v-for="item in taskList" :key="item.id">
              <task-card
                  :taskId="item.taskId"
                  :taskName="item.taskName"
                  :description="item.description"
                  :state="item.state"
                  :publisherId="item.publisherId"
                  :showBtn="true"
                  :timeLimit="item.timeLimit"
                  :address="item.address"
                  :reward="item.reward"
                  :receivable="!curTask.taskId"
              >
              </task-card>
            </el-carousel-item>
          </el-carousel>
        </el-main>
      </el-container>
    </div>
    <div class="curtask">
      <el-container class="tasks">
        <el-header height="30px">
          <span>当前任务：</span>
        </el-header>
        <el-main class="mainFrame">
          <task-card
              v-if="curTask.taskId"
              :taskId="curTask.taskId"
              :taskName="curTask.taskName"
              :description="curTask.description"
              :state="curTask.state"
              :publisherId="curTask.publisherId"
              :performerId="curTask.performerId"
              :timeLimit="curTask.timeLimit"
              :address="curTask.address"
              :reward="curTask.reward"
              :receivable="curTask.taskId"
          >
          </task-card>
        </el-main>
        <el-footer>
          <el-button
              v-if="curTask.taskId"
              type="primary"
              @click="finishTask">
            完成任务
          </el-button>
          <el-button
              v-if="curTask.taskId"
              type="danger"
              @click="stopTaskByPerformer">
            取消任务
          </el-button>
        </el-footer>
      </el-container>
    </div>
  </div>
</template>

<script>
import TaskCard from '../TaskCard'
// import PublishTask from './PublishTask'
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
      
      curTask:{},
      publishFlag:false
    }
  },
  methods: {
    async finishTask() {
      const { data: res } = await this.$http.post("finishTaskPerformer",this.curTask.taskId);
      this.curTask = {}
      this.$router.go(0);//刷新当前页面
    },
    async stopTaskByPerformer() {
      const { data: res } = await this.$http.post("stopTaskByPerformer",this.curTask.taskId);
      this.curTask = {}
      this.$router.go(0);//刷新当前页面
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
    // PublishTask,

  },
  async mounted() {
    const { data: res1 } = await this.$http.post("getAllTasks",JSON.parse(window.sessionStorage.getItem("user")).userId);
    this.taskList = res1.tasks;
    const { data: res2 } = await this.$http.post("getTaskByPerformer",JSON.parse(window.sessionStorage.getItem("user")).userId);
    this.curTask = res2;
    //console.log(res.tasks)
  }
}
</script>

<style lang='less' scoped>
.mainContainer{
  /*background: white;*/
  
  background-color: rgb(125, 199, 153);
  
  /* float:left; */
  width:100%;
  height: 100%;
  display: flex;
}
.tasklist{
  width: 66%;
}
.curtask{
  width:34%
}


</style>