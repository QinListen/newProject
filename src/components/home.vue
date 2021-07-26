<template>
  <div>
    <el-button type="info" @click="logout">退出</el-button>
  </div>
</template>

<script>
export default {
  methods: {
    // 退出功能
    logout() {
      // 先清空token
      window.sessionStorage.clear();
      // 在利用路由跳转到登录页
      this.$router.push("/login");
    },
  },
};
</script>

<style>
</style>