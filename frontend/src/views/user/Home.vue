<template>
  <div class="home">
    <el-row :gutter="20">
      <el-col :span="24"><h2>商城首页</h2></el-col>
      <el-col :span="24">
        <h3>最新公告</h3>
        <ul v-if="announcements.length">
          <li v-for="a in announcements" :key="a.id">
            <router-link :to="'/announcements'">{{ a.title }}</router-link>
          </li>
        </ul>
      </el-col>
      <el-col :span="24"><h3>新品上架</h3></el-col>
      <el-col v-for="p in newList" :key="p.id" :xs="12" :sm="8" :md="6">
        <product-card :product="p" />
      </el-col>
      <el-col :span="24"><h3>销量排行</h3></el-col>
      <el-col v-for="p in rankList" :key="p.id" :xs="12" :sm="8" :md="6">
        <product-card :product="p" />
      </el-col>
      <template v-if="user">
        <el-col :span="24"><h3>猜您想买</h3></el-col>
        <el-col v-for="p in recommendList" :key="p.id" :xs="12" :sm="8" :md="6">
          <product-card :product="p" />
        </el-col>
      </template>
    </el-row>
  </div>
</template>

<script>
import { getNewArrivals, getSalesRank, getRecommend, getAnnouncements } from '@/api/pub'

export default {
  name: 'Home',
  data() {
    return {
      newList: [],
      rankList: [],
      recommendList: [],
      announcements: []
    }
  },
  computed: {
    user() {
      return this.$store.state.user
    }
  },
  created() {
    getNewArrivals(8).then(res => { if (res.data) this.newList = res.data })
    getSalesRank(8).then(res => { if (res.data) this.rankList = res.data })
    getAnnouncements(5).then(res => { if (res.data) this.announcements = res.data })
    if (this.user && this.user.userId) {
      getRecommend(this.user.userId, 8).then(res => { if (res.data) this.recommendList = res.data })
    }
  },
  components: {
    ProductCard: () => import('@/components/ProductCard.vue')
  }
}
</script>

<style scoped>
.home h2 { margin-bottom: 16px; }
.home h3 { margin: 24px 0 12px; }
ul { list-style: none; }
ul li { margin: 6px 0; }
ul a { color: #409EFF; }
</style>
