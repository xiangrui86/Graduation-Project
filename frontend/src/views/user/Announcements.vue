<template>
  <div>
    <h2>最新公告</h2>
    <el-card v-for="a in list" :key="a.id" class="item">
      <h4>{{ a.title }}</h4>
      <p class="time">{{ a.createdAt }}</p>
      <p class="content">{{ a.content }}</p>
    </el-card>
  </div>
</template>

<script>
import { getAnnouncements } from '@/api/pub'

export default {
  name: 'Announcements',
  data() {
    return { list: [] }
  },
  created() {
    getAnnouncements(20).then(res => { if (res.data) this.list = res.data })
  }
}
</script>

<style scoped>
.item { margin-bottom: 16px; }
.time { color: #999; font-size: 12px; }
.content { margin-top: 8px; color: #666; }
</style>
