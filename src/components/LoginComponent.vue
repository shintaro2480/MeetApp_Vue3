<template>
  <div class="register-form">
    <h2>ユーザー登録</h2>
    <form @submit.prevent="registerUser">
      <div>
        <label for="name">名前:</label>
        <input
          type="text"
          id="name"
          v-model="form.name"
          required
        />
      </div>
      
      <div>
        <label for="email">メールアドレス:</label>
        <input
          type="email"
          id="email"
          v-model="form.email"
          required
        />
      </div>
      
      <div>
        <label for="password">パスワード:</label>
        <input
          type="password"
          id="password"
          v-model="form.password"
          required
        />
      </div>
      
      <div>
        <label for="password_confirmation">パスワード確認:</label>
        <input
          type="password"
          id="password_confirmation"
          v-model="form.password_confirmation"
          required
        />
      </div>
      
      <button type="submit">登録</button>
    </form>

    <div v-if="message" class="message">{{ message }}</div>
    <div v-if="error" class="error">{{ error }}</div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      form: {
        name: '',
        email: '',
        password: '',
        password_confirmation: ''
      },
      message: '',
      error: ''
    };
  },
  methods: {
    async registerUser() {
      try {
        await axios.post(
          'http://localhost:8000/api/register',
          this.form,
          { withCredentials: true }
        );
        this.message = '登録が成功しました！';
        this.error = '';
        // 必要であれば登録後の処理（例: ログインページにリダイレクト）
      } catch (error) {
        if (error.response) {
          this.error = error.response.data.message || '登録に失敗しました';
        } else {
          this.error = 'サーバーに接続できません';
        }
        this.message = '';
      }
    }
  }
};
</script>

<style scoped>
.register-form {
  max-width: 400px;
  margin: auto;
}
.message {
  color: green;
}
.error {
  color: red;
}
</style>