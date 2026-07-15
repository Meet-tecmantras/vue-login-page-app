<template>
  <section class="login-card" aria-live="polite">
    <header class="login-card__header">
      <p class="eyebrow">Secure workspace</p>
      <h1>Sign in to continue</h1>
      <p class="lead">Use your corporate credentials or personal email to get started.</p>
    </header>
    <form class="login-card__form" @submit.prevent="handleSubmit" novalidate>
      <div class="form-field" :class="{ 'has-error': emailError }">
        <label for="email">Email address</label>
        <input
          id="email"
          name="email"
          type="email"
          autocomplete="username"
          v-model.trim="form.email"
          placeholder="you@example.com"
        />
        <p v-if="emailError" class="error-message" role="alert">
          {{ emailError }}
        </p>
      </div>
      <div class="form-field" :class="{ 'has-error': passwordError }">
        <div class="password-label">
          <label for="password">Password</label>
          <span class="hint">Minimum 6 characters</span>
        </div>
        <input
          id="password"
          name="password"
          type="password"
          autocomplete="current-password"
          v-model.trim="form.password"
        />
        <p v-if="passwordError" class="error-message" role="alert">
          {{ passwordError }}
        </p>
      </div>
      <label class="checkbox">
        <input type="checkbox" v-model="form.remember" />
        <span>Remember this device</span>
      </label>
      <button type="submit" :disabled="!isFormValid || submitting">
        {{ submitting ? 'Signing in...' : 'Sign in' }}
      </button>
      <p v-if="feedback" class="feedback" :class="feedback.type" role="status">
        {{ feedback.message }}
      </p>
    </form>
  </section>
</template>

<script setup>
import { computed, reactive, ref } from 'vue';

const form = reactive({
  email: '',
  password: '',
  remember: false
});
const submitting = ref(false);
const feedback = ref(null);

const emailError = computed(() => {
  if (!form.email) {
    return 'Email address is required.';
  }
  const pattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  if (!pattern.test(form.email)) {
    return 'Enter a valid email address.';
  }
  return '';
});

const passwordError = computed(() => {
  if (!form.password) {
    return 'Password is required.';
  }
  if (form.password.length < 6) {
    return 'Password must be at least 6 characters.';
  }
  return '';
});

const isFormValid = computed(() => !emailError.value && !passwordError.value);

const handleSubmit = () => {
  if (!isFormValid.value) {
    feedback.value = {
      type: 'error',
      message: 'Please fix the validation issues above.'
    };
    return;
  }

  submitting.value = true;
  feedback.value = null;

  const payload = {
    email: form.email,
    password: '••••••••',
    remember: form.remember,
    timestamp: new Date().toISOString()
  };

  setTimeout(() => {
    console.log('[LoginForm] Submitted payload', payload);
    feedback.value = {
      type: 'success',
      message:
        'Credentials look good! The payload was logged to the console as a placeholder for integration.'
    };
    submitting.value = false;
  }, 450);
};
</script>

<style scoped>
.login-card {
  width: min(420px, 100%);
  background: rgba(15, 23, 42, 0.9);
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 24px;
  padding: 2.5rem;
  box-shadow: 0 25px 55px rgba(2, 6, 23, 0.75);
  backdrop-filter: blur(18px);
}
.eyebrow {
  text-transform: uppercase;
  letter-spacing: 0.4em;
  font-size: 0.72rem;
  margin-bottom: 0.75rem;
  color: #94a3b8;
}
h1 {
  margin: 0;
  font-size: 2rem;
}
.lead {
  margin-top: 0.4rem;
  color: #cbd5f5;
}
.login-card__form {
  margin-top: 1.8rem;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}
.form-field {
  display: flex;
  flex-direction: column;
  gap: 0.35rem;
}
.form-field input {
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 12px;
  padding: 0.85rem 1rem;
  background: rgba(255, 255, 255, 0.02);
  color: #f1f5f9;
  transition: border-color 0.2s ease;
}
.form-field input:focus {
  outline: none;
  border-color: #7c3aed;
  box-shadow: 0 0 0 3px rgba(124, 58, 237, 0.2);
}
.form-field.has-error input {
  border-color: #fb7185;
}
.password-label {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.hint {
  color: #94a3b8;
  font-size: 0.85rem;
}
.error-message {
  font-size: 0.85rem;
  color: #fb7185;
  margin: 0;
}
.checkbox {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.95rem;
  color: #cbd5f5;
}
.checkbox input {
  width: 1rem;
  height: 1rem;
}
button {
  width: 100%;
  padding: 0.9rem 1rem;
  border: none;
  border-radius: 12px;
  font-weight: 600;
  color: #fff;
  background: linear-gradient(135deg, #7c3aed, #4f46e5);
  cursor: pointer;
  transition: opacity 0.2s ease, transform 0.2s ease;
}
button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
button:not(:disabled):active {
  transform: translateY(1px);
}
.feedback {
  margin-top: 0.5rem;
  font-size: 0.9rem;
  line-height: 1.4;
}
.feedback.success {
  color: #4ade80;
}
.feedback.error {
  color: #fb7185;
}
@media (max-width: 480px) {
  .login-card {
    padding: 2rem 1.5rem;
    border-radius: 18px;
  }
}
</style>
