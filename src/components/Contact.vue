<template>
  <section class="c-contact">
    <div class="c-contact__container">
      <div class="c-contact__info">
        <p class="c-contact__description">
          Open to collaboration, consulting, and interesting engineering problems. Reach out directly or use the form below.
        </p>

        <div class="c-contact__links">
          <a href="mailto:tribejustice35@gmail.com" class="c-contact__link">
            tribejustice35@gmail.com
          </a>
          <a href="https://github.com/Aarav2709" target="_blank" rel="noopener noreferrer" class="c-contact__link">
            github.com/Aarav2709
          </a>
          <a href="https://linkedin.com/in/aarav2709" target="_blank" rel="noopener noreferrer" class="c-contact__link">
            linkedin.com/in/aarav2709
          </a>
        </div>
      </div>

      <div class="c-contact__form-wrapper">
        <form @submit.prevent="sendMessage" class="c-contact__form">
          <input
            v-model="formData.name"
            type="text"
            placeholder="Your name"
            class="c-contact__input"
            required
          />
          <input
            v-model="formData.email"
            type="email"
            placeholder="Your email"
            class="c-contact__input"
            required
          />
          <textarea
            v-model="formData.message"
            placeholder="Your message"
            class="c-contact__textarea"
            rows="6"
            required
          ></textarea>
          <button type="submit" class="c-contact__button">
            send message
          </button>
        </form>
      </div>
    </div>
  </section>
</template>

<script lang="ts">
import { ref } from 'vue';

export default {
  name: 'ContactComponent',
  setup() {
    const formData = ref({
      name: '',
      email: '',
      message: ''
    });

    const sendMessage = async () => {
      // Handle form submission here
      console.log('Message sent:', formData.value);
      // Reset form after submission
      formData.value = {
        name: '',
        email: '',
        message: ''
      };
    };

    return {
      formData,
      sendMessage
    }
  }
}
</script>
