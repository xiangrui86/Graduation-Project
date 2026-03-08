<template>
  <el-container class="merchant-layout">
    <el-aside width="200px" class="aside">
      <div class="logo">商家中心</div>
      <el-menu :default-active="$route.path" router>
        <el-menu-item index="/merchant">数据报表</el-menu-item>
        <el-menu-item index="/merchant/products">商品管理</el-menu-item>
        <el-menu-item index="/merchant/orders">交易管理</el-menu-item>
        <el-menu-item index="/merchant/chat">客服消息</el-menu-item>
        <el-menu-item @click="logout"><span>退出登录</span></el-menu-item>
      </el-menu>
    </el-aside>
    <el-main><router-view /></el-main>
  </el-container>
</template>

<script>
export default {
  name: 'MerchantLayout',
  methods: {
    logout() {
      this.$store.dispatch('logout')
      this.$router.push('/login')
    }
  }
}
</script>

<style scoped>
.merchant-layout { height: 100vh; }
.aside { background: #304156; }
.logo { color: #fff; padding: 20px; font-weight: bold; text-align: center; }
.el-menu { border: none; background: #304156; }
.el-menu-item { color: #bfcbd9; }
.el-menu-item:hover, .el-menu-item.is-active { color: #409EFF; background: #263445; }
</style>
