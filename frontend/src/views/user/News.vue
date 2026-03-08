<template>
  <div>
    <h2>系统资讯</h2>
    <el-card v-for="n in list" :key="n.id" class="news-item">
      <h4>{{ n.title }}</h4>
      <p class="time">{{ n.createdAt }}</p>
      <p class="content">{{ n.content }}</p>
    </el-card>
  </div>
</template>

<script>
import { getNews } from '@/api/pub'

export default {
  name: 'News',
  data() {
    return { list: [] }
  },
  created() {
    getNews({ page: 0, size: 20 }).then(res => {
      if (res.data && res.data.content) this.list = res.data.content
      else if (Array.isArray(res.data)) this.list = res.data
    })
  }
}
</script>

<style scoped>
.news-item { margin-bottom: 16px; }
.time { color: #999; font-size: 12px; }
.content { margin-top: 8px; color: #666; }
</style>
