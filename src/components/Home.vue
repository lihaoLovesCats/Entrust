<template>
  <!-- 引入container布局 -->
  <el-container class="home-container">
    <!-- 头部 -->
    <el-header>
      <div>
        <img src="../assets/1.png" class="img-home" alt='logo' />
        <span>Entrust</span>
        <span>满足每一份期待</span>
      </div>
      <el-button type="info" @click="logout">安全退出</el-button>
    </el-header>
    <!-- 主体 -->
    <el-container>
      <!-- 侧边栏 -->
      <el-aside :width="isCollapse?'64px':'200px'">
        <div class="toggle-button" @click="toggleCollapase">{{directicon}}</div>

        <!-- /**
         * * 描述信息.作者是谁
         * ! author: psili
         * ? description: 侧边栏内容
         * @param Home
         * TODO: since: 创建时间  2021-05-21 16:56:14
         **/
         -->
        <div class="avatar-my" 
        @click="changeAvatar()">
        <!-- 待完成更换头像功能 -->
          <el-avatar 
          v-if="!isCollapse"
          src="https://cube.elemecdn.com/0/88/03b0d39583f48206768a7534e55bcpng.png" 
          :fit="'contain'" 
          :size='70'>
          </el-avatar>
        </div>
        <el-menu 
        default-active="1-4-1" 
        class="asideMenu" 
        :collapse="isCollapse">
          <el-menu-item 
          index="1"
          @click="myPageFlag=true" 
          >
            <i class="el-icon-menu"></i>
            <span slot="title">个人信息</span>
          </el-menu-item>
        </el-menu>
        <!-- <div class="avatar-my" @click="myPageFlag=true" >
          <el-avatar 
          src="https://cube.elemecdn.com/0/88/03b0d39583f48206768a7534e55bcpng.png" 
          :fit="'contain'" 
          :size='70'>
          </el-avatar>
        </div> -->
        <!-- <div>
          <span>
            个人信息
          </span>
        </div> -->

        <!-- <el-menu
          background-color="#545c64"
          text-color="#fff"
          active-text-color="#409eff"
          :collapse="isCollapse"
          :collapse-transition="false"
          :router="true"
          :default-active="activePath"
        > -->
          <!-- 一级菜单 -->
          <!-- <el-submenu :index="item.id+''" v-for="item in menuList" :key="item.id">
            <template slot="title">
              <i class="el-icon-location"></i>
              <span>{{item.title}}</span>
            </template> -->
            <!-- 二级菜单 -->
            <!-- <el-menu-item :index="it.path" v-for="it in item.sList" :key="it.id" @click="saveNavState(it.path)">
              <template slot="title">
                <i class="el-icon-location"></i>
                <span>{{it.title}}</span>
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu> -->
      </el-aside>
      <!-- 抽屉 -->
      <el-drawer
        title="个人信息"
        ref="drawerMy"
        :destroyOnClose="true"
        :visible.sync="myPageFlag"
        :direction="'ltr'"
        :before-close="handleClose">
        <my-my :drawer='ObjDrawer'></my-my>
      </el-drawer>
      <!-- 主体内容 -->
      <el-main>
        <component :is="showWhat"> </component>
      </el-main>
    </el-container>
  </el-container>
</template>



<script>
import MyMy from "./MyMy";
import MainPageOuter from "./outer/MainPage"
export default {
  data() {
    return {
      ObjDrawer: this.$refs,
      //菜单列表
      menuList:[],
      isCollapse:false,//伸缩
      activePath:'/welcome',//默认路径
      directicon:'👈',
      myPageFlag:false,//控制个人信息抽屉
    }

  },
//onload 事件
  created() {
    //查询menuList
    this.getMenuList();
    this.activePath = window.sessionStorage.getItem('activePath');//取出session里的path 动态修改activePath
  },
  methods: {
    //安全退出
    logout() {
      window.sessionStorage.clear(); //清除session
      this.$router.push("/login"); //回到首页
    },
    //获取导航菜单方法
    async getMenuList() {
      const {data:res} = await this.$http.get("menus");
      console.log(res);
      if(res.flag !=200) return this.$message.error("获取列表失败！！！");//访问失败的错误信息
      this.menuList = res.menus;//访问成功的数据回填
    },
    //控制伸缩
    toggleCollapase() {
      this.isCollapse = !this.isCollapse;
      if(this.isCollapse)this.directicon = '👉';
      else this.directicon = '👈';
    },
    //保存路径
    saveNavState(activePath){
      window.sessionStorage.setItem('activePath',activePath);//存放在session里
      this.activePath = activePath;
    },
    handleClose(done) {
        this.$confirm('确认关闭？')
          .then(_ => {
            done();
          })
          .catch(_ => {});
    },
    changeAvatar() {
      this.$message.error("更换头像功能还未实现！");
    },
  },
  components: {
    MyMy,
    MainPageOuter,
  },
  computed: {
    showWhat() {
      //TODO补全逻辑根据当前用户角色显示不同组件
      return 'MainPageOuter'
    }
  }

};
</script>

<style lang='less' scoped>
.home-container {
  height: 100%;
}
.el-header {
  background-color: #742eb4;
  display: flex;
  justify-content: space-between; //左右贴边
  padding-left: 0%; //左边界
  align-items: center;
  color: #fff;
  font-size: 20px;
  > div {
    display: flex;
    align-items: center;
    span {
      margin-left: 50px;
    }
    
  }
}
.el-aside {
  background-color: #f8f8f8;
  .el-menu{
    border-right: none;
  }
}
// .asideMenu:not(.el-menu--collapse) {
//     width: 200px;
//     //min-height: 400px;
// }
.avatar-my{
  //display: flex;
  //width: 10px;
  //align-items: center;
  padding-left: 26%;
  padding-top: 0%;
}
.el-main {
  background-color: #eaedf1;
  padding: 0;;
}
.img-home {
  width: 55px;
  height: 55px;
}
//|||按钮样式
.toggle-button{
  background-color: #4A5064;
  font-size: 10px;
  line-height: 24px;
  color: #fff;
  text-align: center;
  letter-spacing: 0.2em;
  cursor: pointer;// 显示小手
}
</style>