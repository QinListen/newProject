<template>
  <el-container class="home-container">
    <!-- 头部 -->
    <el-header>
      <span>Listen第一个后台管理系统</span>
      <el-button type="info" @click="logout">退出</el-button>
    </el-header>
    <!-- 页面 -->
    <el-container>
      <!-- 侧边栏 -->
      <el-aside :width="isClass ? '64px' : '200px'">
        <div class="toggle-button" @click="toggleCollapce">☰</div>
        <!-- 侧边栏菜单 -->
        <el-menu
          background-color="#313743"
          text-color="#fff"
          active-text-color="#409EFF"
          :unique-opened="true"
          :collapse="isClass"
          :collapse-transition="false"
          :router="true"
          :default-active="activePath"
        >
          <el-submenu
            :index="item.id + ''"
            v-for="item in menulist"
            :key="item.id"
          >
            <template slot="title">
              <i :class="iconObj[item.id]"></i>
              <span>{{ item.authName }}</span>
            </template>

            <!-- 二级菜单 -->
            <el-menu-item
              :index="'/' + subItem.path"
              v-for="subItem in item.children"
              :key="subItem.id"
              @click="saveNavState('/' + subItem.path)"
            >
              <template slot="title">
                <i class="el-icon-menu"></i>
                <span>{{ subItem.authName }}</span>
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <!-- 右侧主体 -->
      <el-main>
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data() {
    return {
      menulist: [],
      iconObj: {
        125: "iconfont icon-user",
        103: "iconfont icon-tijikongjian",
        101: "iconfont icon-shangpin",
        102: "iconfont icon-danju",
        145: "iconfont icon-baobiao",
      },
      isClass: false,
      // 被激活的链接地址
      activePath: "",
    };
  },
  created() {
    this.getMenuList();
    this.activePath = window.sessionStorage.getItem("activePath");
  },
  methods: {
    // 退出功能
    logout() {
      // 先清空token
      window.sessionStorage.clear();
      // 在利用路由跳转到登录页
      this.$router.push("/login");
    },
    // 获取所有菜单
    async getMenuList() {
      const { data: res } = await this.$http.get("menus");
      if (res.meta.status !== 200) return this.$message.error(res.meta.msg);
      this.menulist = res.data;
    },
    // 点击按钮切换菜单
    toggleCollapce() {
      this.isClass = !this.isClass;
    },
    // 保存链接的激活状态
    saveNavState(activePath) {
      window.sessionStorage.setItem("avtivePath", activePath);
      this.activePath = activePath;
    },
  },
};
</script>

<style lang="less" scoped>
.home-container {
  height: 100%;
}
.el-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #363d40;
}
.el-header span {
  color: #fff;
}
.el-aside {
  background-color: #313743;
  .el-menu {
    border-right: none;
  }
}
.el-main {
  background-color: #e9edf1;
}
.iconfont {
  margin-right: 10px;
}
.toggle-button {
  background-color: #4a5064;
  color: #fff;
  font-size: 10px;
  line-height: 24px;
  text-align: center;
  letter-spacing: 0.2em;
  cursor: pointer;
}
</style>