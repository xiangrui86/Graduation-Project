<template>
  <div v-if="product" class="detail">
    <el-row :gutter="24">
      <el-col :span="10">
        <img v-if="product.image" :src="product.image" class="main-img" alt="">
        <div v-else class="main-img placeholder">暂无图片</div>
      </el-col>
      <el-col :span="14">
        <h1>{{ product.name }}</h1>
        <p class="price">¥ {{ product.price }}</p>
        <p class="desc">{{ product.description || '暂无描述' }}</p>
        <el-input-number v-model="quantity" :min="1" :max="product.stock" />
        <div class="actions">
          <el-button type="primary" @click="addCart">加入购物车</el-button>
          <el-button @click="addFavorite">收藏</el-button>
        </div>
      </el-col>
    </el-row>
    <el-divider>商品评价</el-divider>
    <div v-for="r in reviews" :key="r.id" class="review">
      <span>用户{{ r.userId }}</span> <el-rate v-model="r.rating" disabled /> {{ r.content }}
    </div>
    <el-button v-if="canReview" type="text" @click="showReviewDialog = true">我要评价</el-button>
    <el-dialog title="评价" :visible.sync="showReviewDialog" width="400px">
      <el-form :model="reviewForm">
        <el-form-item label="评分">
          <el-rate v-model="reviewForm.rating" />
        </el-form-item>
        <el-form-item label="内容">
          <el-input v-model="reviewForm.content" type="textarea" />
        </el-form-item>
      </el-form>
      <span slot="footer">
        <el-button @click="showReviewDialog = false">取消</el-button>
        <el-button type="primary" @click="submitReview">提交</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import { getProduct, getReviews } from '@/api/pub'
import { addCart, addFavorite, addReview } from '@/api/user'

export default {
  name: 'ProductDetail',
  data() {
    return {
      product: null,
      quantity: 1,
      reviews: [],
      showReviewDialog: false,
      reviewForm: { rating: 5, content: '' }
    }
  },
  computed: {
    canReview() {
      return this.$store.state.user
    }
  },
  created() {
    const id = this.$route.params.id
    getProduct(id).then(res => {
      if (res.data) this.product = res.data
    })
    getReviews(id).then(res => {
      if (res.data && res.data.content) this.reviews = res.data.content
      else if (Array.isArray(res.data)) this.reviews = res.data
    })
  },
  methods: {
    addCart() {
      if (!this.$store.state.user) {
        this.$router.push('/login')
        return
      }
      addCart({ productId: this.product.id, quantity: this.quantity }).then(res => {
        if (res.code === 200) this.$message.success('已加入购物车')
        else this.$message.error(res.message || '失败')
      })
    },
    addFavorite() {
      if (!this.$store.state.user) {
        this.$router.push('/login')
        return
      }
      addFavorite({ productId: this.product.id }).then(res => {
        if (res.code === 200) this.$message.success('已收藏')
        else this.$message.error(res.message || '失败')
      })
    },
    submitReview() {
      addReview({
        productId: this.product.id,
        rating: this.reviewForm.rating,
        content: this.reviewForm.content
      }).then(res => {
        if (res.code === 200) {
          this.$message.success('评价成功')
          this.showReviewDialog = false
          this.reviews.unshift(res.data)
        }
      })
    }
  }
}
</script>

<style scoped>
.detail .main-img { width: 100%; max-height: 400px; object-fit: contain; }
.placeholder { height: 300px; background: #f0f0f0; display: flex; align-items: center; justify-content: center; color: #999; }
.price { font-size: 24px; color: #f56c6c; margin: 12px 0; }
.desc { color: #666; margin-bottom: 16px; }
.actions { margin-top: 16px; }
.review { margin: 8px 0; padding: 8px; background: #f9f9f9; border-radius: 4px; }
</style>
