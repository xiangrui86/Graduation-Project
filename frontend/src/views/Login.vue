<template>
  <div class="login-page">
    <el-card class="login-card">
      <h2>体育用品电商系统登录</h2>
      <el-form :model="form" label-width="80px">
        <el-form-item label="用户名">
          <el-input v-model="form.username" placeholder="请输入用户名" />
        </el-form-item>
        <el-form-item label="密码">
          <el-input
            v-model="form.password"
            type="password"
            placeholder="请输入密码"
            show-password
          />
        </el-form-item>
        <el-form-item>
          <el-button type="primary" :loading="loading" @click="handleLogin"
            >登录</el-button
          >
          <el-button @click="showRegister = true">注册</el-button>
        </el-form-item>
      </el-form>
    </el-card>

    <el-dialog title="用户注册" :visible.sync="showRegister" width="400px">
      <el-form :model="regForm" label-width="80px">
        <el-form-item label="用户名">
          <el-input v-model="regForm.username" placeholder="用户名" />
        </el-form-item>
        <el-form-item label="密码">
          <el-input
            v-model="regForm.password"
            type="password"
            placeholder="密码"
          />
        </el-form-item>
        <el-form-item label="昵称">
          <el-input v-model="regForm.nickname" placeholder="昵称" />
        </el-form-item>
      </el-form>
      <span slot="footer">
        <el-button @click="showRegister = false">取消</el-button>
        <el-button type="primary" :loading="regLoading" @click="handleRegister"
          >注册</el-button
        >
      </span>
    </el-dialog>
  </div>
</template>

<script>
import { login, register } from "@/api/auth";

export default {
  name: "Login",
  data() {
    return {
      form: { username: "", password: "" },
      regForm: { username: "", password: "", nickname: "" },
      loading: false,
      regLoading: false,
      showRegister: false,
    };
  },
  methods: {
    async handleLogin() {
      this.loading = true;
      try {
        const res = await login(this.form);
        if (res.code !== 200) {
          this.$message.error(res.message || "登录失败");
          return;
        }
        this.$store.dispatch("login", {
          token: res.data.token,
          userId: res.data.userId,
          username: res.data.username,
          role: res.data.role,
          nickname: res.data.nickname,
          merchantId: res.data.merchantId,
        });
        if (res.data.role === "ADMIN") this.$router.push("/admin");
        else if (res.data.role === "MERCHANT") this.$router.push("/merchant");
        else this.$router.push("/");
      } catch (e) {
        this.$message.error(e.response?.data?.message || "登录失败");
      } finally {
        this.loading = false;
      }
    },
    async handleRegister() {
      this.regLoading = true;
      try {
        const res = await register(this.regForm);
        if (res.code !== 200) {
          this.$message.error(res.message || "注册失败");
          return;
        }
        this.$message.success("注册成功，请登录");
        this.showRegister = false;
      } catch (e) {
        this.$message.error(e.response?.data?.message || "注册失败");
      } finally {
        this.regLoading = false;
      }
    },
  },
};
</script>

<style scoped>
.login-page {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  background-image: linear-gradient(
      135deg,
      rgba(102, 126, 234, 0.8) 0%,
      rgba(118, 75, 162, 0.8) 100%
    ),
    url("https://images.unsplash.com/photo-1557821552-17105176677c?w=1200&h=800&fit=crop");
  background-size: cover;
  background-position: center;
  background-attachment: fixed;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}
.login-card {
  width: 400px;
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  border-radius: 12px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2);
}
.login-card h2 {
  text-align: center;
  margin-bottom: 20px;
  color: #333;
  font-weight: 600;
}
</style>
