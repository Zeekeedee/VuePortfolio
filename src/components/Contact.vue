<template>
  <div class="c-contact__info">
    <p class="c-contact__description">
      Open to collaboration, consulting, and interesting engineering problems. Reach out directly or
      use the form below.
    </p>
  </div>

  <div class="c-contact__form-wrapper" @click.stop>
    <form @submit.prevent="sendMessage" class="c-contact__form">
      <input
        v-model="formData.name"
        type="text"
        placeholder="Your name"
        class="c-contact__input"
        :disabled="status === 'loading'"
        required
      />
      <input
        v-model="formData.email"
        type="email"
        placeholder="Your email"
        class="c-contact__input"
        :disabled="status === 'loading'"
        required
      />
      <textarea
        v-model="formData.message"
        placeholder="Your message"
        class="c-contact__textarea"
        rows="4"
        :disabled="status === 'loading'"
        required
      ></textarea>
      <div class="c-contact__form-footer">
        <p v-if="status === 'success'" class="c-contact__status c-contact__status--success">
          Message sent — I'll be in touch.
        </p>
        <p v-else-if="status === 'error'" class="c-contact__status c-contact__status--error">
          {{ errorMessage }}
        </p>
        <button
          type="submit"
          class="c-contact__button"
          :class="{ 'c-contact__button--loading': status === 'loading' }"
          :disabled="status === 'loading'"
        >
          {{ status === 'loading' ? 'sending…' : 'send message' }}
        </button>
      </div>
    </form>
  </div>
</template>

<script lang="ts">
import { ref } from 'vue'
import emailjs from '@emailjs/browser'

const EMAILJS_SERVICE_ID = import.meta.env.VITE_EMAILJS_SERVICE_ID as string
const EMAILJS_TEMPLATE_ID = import.meta.env.VITE_EMAILJS_TEMPLATE_ID as string
const EMAILJS_PUBLIC_KEY = import.meta.env.VITE_EMAILJS_PUBLIC_KEY as string

type Status = 'idle' | 'loading' | 'success' | 'error'

export default {
  name: 'ContactComponent',
  setup() {
    const formData = ref({ name: '', email: '', message: '' })
    const status = ref<Status>('idle')
    const errorMessage = ref('')

    const sendMessage = async () => {
      status.value = 'loading'
      errorMessage.value = ''

      try {
        await emailjs.send(
          EMAILJS_SERVICE_ID,
          EMAILJS_TEMPLATE_ID,
          {
            from_name: formData.value.name,
            from_email: formData.value.email,
            message: formData.value.message,
          },
          EMAILJS_PUBLIC_KEY,
        )
        status.value = 'success'
        formData.value = { name: '', email: '', message: '' }
      } catch {
        status.value = 'error'
        errorMessage.value = 'Something went wrong. Try emailing me directly.'
      }
    }

    return { formData, status, errorMessage, sendMessage }
  },
}
</script>
