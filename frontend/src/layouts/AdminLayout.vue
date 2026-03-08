<template>
  <el-container class="admin-layout">
    <el-aside width="200px" class="aside">
      <div class="logo">管理后台</div>
      <el-menu :default-active="$route.path" router>
        <el-menu-item index="/admin">数据报表</el-menu-item>
        <el-menu-item index="/admin/users">用户管理</el-menu-item>
        <el-menu-item index="/admin/merchants">商家管理</el-menu-item>
        <el-menu-item index="/admin/categories">分类管理</el-menu-item>
        <el-menu-item index="/admin/products">商品管理</el-menu-item>
        <el-menu-item index="/admin/orders">交易管理</el-menu-item>
        <el-menu-item index="/admin/news">资讯管理</el-menu-item>
        <el-menu-item @click="logout"><span>退出登录</span></el-menu-item>
      </el-menu>
    </el-aside>
    <el-main><router-view /></el-main>
  </el-container>
</template>

<script>
export default {
  name: 'AdminLayout',
  methods: {
    logout() {
      this.$store.dispatch('logout')
      this.$router.push('/login')
    }
  }
}
</script>

<style scoped>
.admin-layout { height: 100vh; }
.aside { background: #304156; }
.logo { color: #fff; padding: 20px; font-weight: bold; text-align: center; }
.el-menu { border: none; background: #304156; }
.el-menu-item { color: #bfcbd9; }
.el-menu-item:hover, .el-menu-item.is-active { color: #409EFF; background: #263445; }
</style>
