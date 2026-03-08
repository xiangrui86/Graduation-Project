<template>
  <div>
    <h2>数据报表</h2>
    <el-row :gutter="20">
      <el-col :span="8">
        <el-card><div class="num">{{ data.userCount || 0 }}</div><div>用户数</div></el-card>
      </el-col>
      <el-col :span="8">
        <el-card><div class="num">{{ data.productCount || 0 }}</div><div>商品数</div></el-card>
      </el-col>
      <el-col :span="8">
        <el-card><div class="num">{{ data.orderCount || 0 }}</div><div>订单数</div></el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import { getReport } from '@/api/admin'

export default {
  name: 'AdminDashboard',
  data() {
    return { data: {} }
  },
  created() {
    getReport().then(res => { if (res.data) this.data = res.data })
  }
}
</script>

<style scoped>
.num { font-size: 28px; font-weight: bold; color: #409EFF; }
</style>
