<template>
  <div class="user-layout">
    <header class="header">
      <router-link to="/" class="logo">商城首页</router-link>
      <nav>
        <router-link to="/">首页</router-link>
        <router-link to="/products">全部商品</router-link>
        <router-link to="/new">新品上架</router-link>
        <router-link to="/recommend">猜您想买</router-link>
        <router-link to="/rank">销量排行</router-link>
        <router-link to="/cart">购物车</router-link>
        <router-link to="/favorite">我的收藏</router-link>
        <router-link to="/orders">我的订单</router-link>
        <router-link to="/news">系统资讯</router-link>
        <router-link to="/announcements">最新公告</router-link>
        <span class="user">{{ user && user.nickname }}</span>
        <el-button size="small" @click="logout">退出</el-button>
      </nav>
    </header>
    <main class="main">
      <router-view />
    </main>
  </div>
</template>

<script>
export default {
  name: 'UserLayout',
  computed: {
    user() {
      return this.$store.state.user
    }
  },
  methods: {
    logout() {
      this.$store.dispatch('logout')
      this.$router.push('/login')
    }
  }
}
</script>

<style scoped>
.user-layout { min-height: 100vh; background: #f5f5f5; }
.header { background: #fff; padding: 12px 24px; box-shadow: 0 1px 4px rgba(0,0,0,.08); display: flex; align-items: center; justify-content: space-between; }
.logo { font-size: 18px; font-weight: bold; color: #409EFF; text-decoration: none; }
nav { display: flex; align-items: center; gap: 16px; flex-wrap: wrap; }
nav a { color: #333; text-decoration: none; }
nav a.router-link-active { color: #409EFF; }
.user { color: #666; margin-right: 8px; }
.main { padding: 20px; max-width: 1200px; margin: 0 auto; }
</style>
