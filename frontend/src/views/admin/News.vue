<template>
  <div>
    <h2>资讯管理</h2>
    <el-table :data="list" border>
      <el-table-column prop="id" label="ID" width="80" />
      <el-table-column prop="title" label="标题" />
      <el-table-column prop="type" label="类型" width="100" />
      <el-table-column prop="published" label="发布" width="80">
        <template slot-scope="scope">
          <el-tag :type="scope.row.published ? 'success' : 'info'">{{ scope.row.published ? '是' : '否' }}</el-tag>
        </template>
      </el-table-column>
      <el-table-column prop="createdAt" label="创建时间" />
    </el-table>
  </div>
</template>

<script>
import { getNews } from '@/api/admin'

export default {
  name: 'AdminNews',
  data() {
    return { list: [] }
  },
  created() {
    getNews().then(res => { if (res.data) this.list = res.data })
  }
}
</script>
