<template>
  <div>
    <h2>商品管理</h2>
    <el-table :data="list" border>
      <el-table-column prop="id" label="ID" width="80" />
      <el-table-column prop="name" label="名称" />
      <el-table-column prop="price" label="价格" width="100" />
      <el-table-column prop="stock" label="库存" width="80" />
      <el-table-column prop="sales" label="销量" width="80" />
      <el-table-column prop="onSale" label="上架" width="80">
        <template slot-scope="scope">
          <el-tag :type="scope.row.onSale ? 'success' : 'info'">{{ scope.row.onSale ? '是' : '否' }}</el-tag>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import { getProducts } from '@/api/admin'

export default {
  name: 'AdminProducts',
  data() {
    return { list: [] }
  },
  created() {
    getProducts({ page: 0, size: 50 }).then(res => {
      if (res.data && res.data.content) this.list = res.data.content
    })
  }
}
</script>
