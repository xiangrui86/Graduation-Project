<template>
  <div>
    <h2>客服消息</h2>
    <p>请输入用户ID进行对话：</p>
    <el-input v-model="targetUserId" placeholder="用户ID" style="width:200px;margin-right:8px" />
    <el-button type="primary" @click="loadChat">加载对话</el-button>
    <div v-if="targetUserId" class="chat-box">
      <div v-for="m in messages" :key="m.id" :class="['msg', m.senderId === meId ? 'me' : 'other']">
        {{ m.content }}
      </div>
      <el-input v-model="inputMsg" type="textarea" placeholder="输入消息" style="margin-top:12px" />
      <el-button type="primary" style="margin-top:8px" @click="send">发送</el-button>
    </div>
  </div>
</template>

<script>
import { getChatList, sendChat } from '@/api/merchant'

export default {
  name: 'MerchantChat',
  data() {
    return {
      targetUserId: '',
      messages: [],
      inputMsg: '',
      meId: null
    }
  },
  created() {
    this.meId = this.$store.state.user && this.$store.state.user.userId
  },
  methods: {
    loadChat() {
      if (!this.targetUserId) return
      getChatList(this.targetUserId).then(res => {
        if (res.data) this.messages = res.data
      })
    },
    send() {
      if (!this.inputMsg.trim()) return
      sendChat({ receiverId: Number(this.targetUserId), content: this.inputMsg }).then(res => {
        if (res.code === 200) {
          this.messages.push(res.data)
          this.inputMsg = ''
        }
      })
    }
  }
}
</script>

<style scoped>
.chat-box { margin-top: 16px; padding: 16px; border: 1px solid #eee; border-radius: 8px; }
.msg { margin: 6px 0; padding: 8px; border-radius: 4px; max-width: 80%; }
.msg.me { background: #409EFF; color: #fff; margin-left: auto; }
.msg.other { background: #f0f0f0; }
</style>
