<template>
  <div class="login-container">
    <form class="login-form" @submit.prevent="handleLogin">
      <h1>Entrar</h1>

      <div v-if="errorMessage" class="error-message">{{ errorMessage }}</div>

      <div class="field">
        <label for="email">Email</label>
        <input
          id="email"
          v-model="email"
          type="email"
          placeholder="seu@email.com"
          required
          autocomplete="email"
        />
      </div>

      <div class="field">
        <label for="password">Senha</label>
        <input
          id="password"
          v-model="password"
          type="password"
          placeholder="••••••••"
          required
          autocomplete="current-password"
        />
      </div>

      <button type="submit" :disabled="loading">
        {{ loading ? 'Entrando...' : 'Entrar' }}
      </button>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import { useAuthStore } from '../stores/auth';

const router = useRouter();
const authStore = useAuthStore();

const email = ref('');
const password = ref('');
const loading = ref(false);
const errorMessage = ref('');

async function handleLogin() {
  loading.value = true;
  errorMessage.value = '';
  try {
    await authStore.login(email.value, password.value);
    router.push('/');
  } catch (err) {
    errorMessage.value =
      err.response?.data?.detail ??
      'Erro ao entrar. Verifique suas credenciais.';
  } finally {
    loading.value = false;
  }
}
</script>

<style scoped>
.login-container {
  width: 100%;
  min-height: 70vh;
  min-height: 70dvh;

  display: flex;
  align-items: center;
  justify-content: center;

  padding: 20px;
  box-sizing: border-box;

}

.login-form {
  width: 100%;
  max-width: 360px;

  padding: 28px 22px;

  box-sizing: border-box;

  background: #ffffff;
  border-radius: 18px;

  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.18);
}

.login-form h1 {
  margin: 0 0 8px;

  color: #1f2937;

  font-size: 26px;
  font-weight: 700;

  text-align: center;
}

.login-form p {
  margin: 0 0 24px;

  color: #6b7280;

  font-size: 14px;
  line-height: 1.5;

  text-align: center;
}

.error-message {
  width: 100%;

  margin-bottom: 18px;
  padding: 11px 12px;

  box-sizing: border-box;

  border-radius: 9px;
  border: 1px solid #fecaca;

  background: #fef2f2;
  color: #dc2626;

  font-size: 13px;
  line-height: 1.4;

  text-align: center;
}

.field {
  width: 100%;
  margin-bottom: 17px;
}

.field label {
  display: block;

  margin-bottom: 7px;

  color: #374151;

  font-size: 14px;
  font-weight: 600;
}

.field input {
  width: 100%;
  height: 48px;

  padding: 0 14px;

  box-sizing: border-box;

  border: 1.5px solid #d1d5db;
  border-radius: 10px;

  outline: none;

  background: #ffffff;
  color: #111827;

  font-size: 16px;

  transition:
    border-color 0.2s,
    box-shadow 0.2s;
}

.field input::placeholder {
  color: #9ca3af;
}

.field input:focus {
  border-color: #ffb1cb;

  box-shadow: 0 0 0 3px rgba(124, 58, 237, 0.12);
}

.login-form button {
  width: 100%;
  height: 48px;

  margin-top: 5px;

  border: none;
  border-radius: 10px;

  background: #ffb1cb;
  color: #ffffff;

  font-size: 16px;
  font-weight: 600;

  cursor: pointer;

  transition:
    background 0.2s,
    transform 0.1s;
}

.login-form button:active:not(:disabled) {
  transform: scale(0.98);
  background: #ffb1cb;
}

.login-form button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.register-link {
  margin-top: 20px !important;
  margin-bottom: 0 !important;

  font-size: 14px !important;
  text-align: center;
}

.register-link a {
  color: #ffb1cb;

  font-weight: 600;
  text-decoration: none;
}

@media (max-width: 360px) {
  .login-container {
    padding: 15px;
  }

  .login-form {
    padding: 24px 18px;
  }

  .login-form h1 {
    font-size: 24px;
  }

  .field input,
  .login-form button {
    height: 46px;
  }
}

</style>