<template>
  <div>
    <h2>用户管理</h2>
    <el-table :data="list" border>
      <el-table-column prop="id" label="ID" width="80" />
      <el-table-column prop="username" label="用户名" />
      <el-table-column prop="nickname" label="昵称" />
      <el-table-column prop="role" label="角色" width="100" />
      <el-table-column prop="enabled" label="启用" width="80">
        <template slot-scope="scope">
          <el-tag :type="scope.row.enabled ? 'success' : 'info'">{{ scope.row.enabled ? '是' : '否' }}</el-tag>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import { getUsers } from '@/api/admin'

export default {
  name: 'AdminUsers',
  data() {
    return { list: [] }
  },
  created() {
    getUsers().then(res => {
      if (res.data && res.data.content) this.list = res.data.content
      else if (Array.isArray(res.data)) this.list = res.data
    })
  }
}
</script>
