<template>
  <el-card class="product-card" shadow="hover" @click.native="goDetail">
    <img v-if="product.image" :src="product.image" class="img" alt="">
    <div v-else class="img placeholder">暂无图片</div>
    <div class="info">
      <div class="name">{{ product.name }}</div>
      <div class="price">¥ {{ product.price }}</div>
      <el-button v-if="showCart" size="small" type="primary" @click.stop="addCart">加购物车</el-button>
    </div>
  </el-card>
</template>

<script>
import { addCart } from '@/api/user'

export default {
  name: 'ProductCard',
  props: {
    product: { type: Object, required: true },
    showCart: { type: Boolean, default: true }
  },
  methods: {
    goDetail() {
      this.$router.push(`/products/${this.product.id}`)
    },
    addCart() {
      const user = this.$store.state.user
      if (!user) {
        this.$router.push('/login')
        return
      }
      addCart({ productId: this.product.id, quantity: 1 }).then(res => {
        if (res.code === 200) this.$message.success('已加入购物车')
        else this.$message.error(res.message || '失败')
      }).catch(() => this.$message.error('失败'))
    }
  }
}
</script>

<style scoped>
.product-card { cursor: pointer; margin-bottom: 16px; }
.img { width: 100%; height: 160px; object-fit: cover; }
.placeholder { background: #f0f0f0; display: flex; align-items: center; justify-content: center; color: #999; }
.info { padding-top: 8px; }
.name { font-size: 14px; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
.price { color: #f56c6c; font-weight: bold; margin: 4px 0; }
</style>
