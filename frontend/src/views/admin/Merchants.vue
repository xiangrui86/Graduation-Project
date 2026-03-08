<template>
  <div>
    <h2>商家管理</h2>
    <el-table :data="list" border>
      <el-table-column prop="id" label="ID" width="80" />
      <el-table-column prop="name" label="名称" />
      <el-table-column prop="description" label="描述" />
      <el-table-column prop="enabled" label="启用" width="80">
        <template slot-scope="scope">
          <el-tag :type="scope.row.enabled ? 'success' : 'info'">{{ scope.row.enabled ? '是' : '否' }}</el-tag>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import { getMerchants } from '@/api/admin'

export default {
  name: 'AdminMerchants',
  data() {
    return { list: [] }
  },
  created() {
    getMerchants().then(res => { if (res.data) this.list = res.data })
  }
}
</script>
