<template>
  <div>
    用户列表
    <!--面包屑导航-->
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>权限管理</el-breadcrumb-item>
      <el-breadcrumb-item>用户列表</el-breadcrumb-item>
    </el-breadcrumb>
    <!--用户列表主体-->
    <el-card>
      <el-row :gutter="25"
        ><!--间隙-->
        <!--搜索区域   -->
        <!--搜索添加-->
        <el-col :span="10">
          <el-input
            placeholder="请输入搜索内容"
            v-model="queryInfo.query"
            clearable
            @clear="getUserList"
          >
            <el-button
              slot="append"
              icon="el-icon-search"
              @click="getUserList"
            ></el-button>
          </el-input>
        </el-col>
        <el-col :span="4">
          <!--搜索按钮-->
          <el-button type="primary" @click="addDialogVisible = true"
            >添加用户</el-button
          >
        </el-col>
      </el-row>
      <!--用户列表 border边框 stripe隔行变色-->
      <el-table :data="userList" border stripe>
        <el-table-column type="index"></el-table-column
        ><!--索引列-->
        <el-table-column label="用户名" prop="username"></el-table-column>
        <el-table-column label="密码" prop="password"></el-table-column>
        <el-table-column label="角色" prop="rule"></el-table-column>
        <el-table-column label="状态" prop="state">
          <!--作用域插槽-->
          <template slot-scope="scope">
            <el-switch
              v-model="scope.row.state"
              @change="userStateChanged(scope.row)"
            ></el-switch>
          </template>
        </el-table-column>
        <el-table-column label="操作">
          <template>
            <!-- 修改 -->
            <el-button
              type="primary"
              icon="el-icon-edit"
              size="mini"
            ></el-button>
            <!-- 删除 -->
            <el-button
              type="danger"
              icon="el-icon-delete"
              size="mini"
            ></el-button>
            <!-- 权限 -->
            <el-tooltip
              effect="dark"
              content="分配权限"
              placement="top-start"
              :enterable="false"
            >
              <el-button
                type="warning"
                icon="el-icon-setting"
                size="mini"
              ></el-button>
            </el-tooltip>
          </template>
        </el-table-column>
      </el-table>
      <!--分页组件 1.每页最大变化数量 2.当前变化数 layout功能组件-->
      <div>
        <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="queryInfo.pageNum"
          :page-sizes="[1, 2, 5, 100]"
          :page-size="queryInfo.pageSize"
          layout="total, sizes, prev, pager, next, jumper"
          :total="total"
        >
        </el-pagination>
      </div>
    </el-card>
    <!--新增用户区域-->
    <el-dialog
      title="添加用户"
      :visible.sync="addDialogVisible"
      width="50%"
      @close="addDialogClosed"
    >
      <el-form
        :model="addForm"
        :rules="addFormRules"
        ref="addFormRef"
        label-width="70px"
      >
        <el-form-item label="用户名" prop="username">
          <el-input v-model="addForm.username"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="password">
          <el-input v-model="addForm.password"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="addDialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="addUser">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>
<script>
export default {
  created() {
    this.getUserList();
  },
  data() {
    return {
      //查询信息实体
      queryInfo: {
        query: "", //查询信息
        pageNum: 1,
        pageSize: 5,
      },
      userList: [], //用户列表
      total: 0, //总记录数
      addDialogVisible: false, //对话框状态
      addForm: {
        username: "",
        password: "",
      }, //添加表单信息
      //表单验证
      addFormRules: {
        username: [
          { required: true, message: "请输入用户名", trigger: "blur" },
          { min: 5, max: 8, message: "长度在 5 到 8 个字符", trigger: "blur" },
        ],
        password: [
          { required: true, message: "请输入密码", trigger: "blur" },
          { min: 6, max: 8, message: "长度在 6 到 8 个字符", trigger: "blur" },
        ],
      },
    };
  },
  methods: {
    async getUserList() {
      const { data: res } = await this.$http.get("alluser", {
        params: this.queryInfo,
      });
      this.userList = res.data;
      this.total = res.numbers;
    },
    //最大数
    handleSizeChange(newSize) {
      this.queryInfo.pageSize = newSize;
      this.getUserList();
    },
    //pageNum的触发
    handleCurrentChange(newPage) {
      this.queryInfo.pageNum = newPage;
      this.getUserList();
    },
    //修改states
    async userStateChanged(userInfo) {
      const { data: res } = await this.$http.put(
        `userstate?id=${userInfo.id}&state=${userInfo.state}`
      );
      if (res != "success") {
        userInfo.state = !userInfo.state;
        return this.$message.error("操作失败！！！");
      }
      this.$message.success("操作成功！！！");
    },
    //监听添加用户
    addDialogClosed(){
        this.$refs.addFormRef.resetFields();
    },
    addUser(){
        this.$refs.addFormRef.validate(async valid=>{
            if(!valid) return;
            const {data:res} = await this.$http.post('/addUser',this.addForm);
            if(res!="success"){
                return this.$message.error("操作失败！！！");
            }
            this.$message.success("操作成功！！！");
            this.addDialogVisible = false;
            this.getUserList();

        })
    }
  },
};
</script>
<style lang="less" scoped>
.el-breadcrumb {
  margin-bottom: 15px;
  font-size: 17px;
}
</style>
 