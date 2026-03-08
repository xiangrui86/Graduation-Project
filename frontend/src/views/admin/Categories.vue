<template>
  <div>
    <h2>分类管理</h2>
    <el-table :data="list" border>
      <el-table-column prop="id" label="ID" width="80" />
      <el-table-column prop="name" label="名称" />
      <el-table-column prop="parentId" label="父级ID" width="100" />
      <el-table-column prop="sortOrder" label="排序" width="80" />
    </el-table>
  </div>
</template>

<script>
import { getCategories } from '@/api/admin'

export default {
  name: 'AdminCategories',
  data() {
    return { list: [] }
  },
  created() {
    getCategories().then(res => { if (res.data) this.list = res.data })
  }
}
</script>
