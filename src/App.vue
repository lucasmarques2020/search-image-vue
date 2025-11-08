<script setup>
import RenderImage from './components/RenderImage.vue'
import FormInput from './components/FormInput.vue'
</script>

<template>
  <main class="container">
    <section class="container-wrapper">
      <h1 class="text-white">Crie imagens de mockup instantaneamente</h1>
      <h3 class="text-white">
        Escolha o tamanho, defina o tema e obtenha imagens aleatórias em segundos para protótipos, sites ou apps.
      </h3>
      <FormInput @renderImage="renderImage" />
      <RenderImage :image="image" />
    </section>
  </main>
</template>

<script>
export default {
  data() {
    return {
      image: ''
    }
  },
  methods: {
    renderImage(img) {
      this.image = img
    }
  }
}
</script>

<style scoped>
.container {
  background: linear-gradient(
    180deg,
    rgba(0, 0, 0, 0.76) 1%,
    rgba(0, 0, 0, 0.84) 50%,
    rgba(0, 0, 0, 0.97)
  );
  height: 100%;
  width: 100%;
}

.container h1 {
  font-size: 2.2rem;
  color: #fff;
}

.container h3 {
  font-size: 1.25rem;
  color: #fff;
}

.container::before {
  background-image: url(https://fastly.picsum.photos/id/115/1280/720.jpg?hmac=GOG9AmYyg9CR0FiDq-hEVsqLBO9rLH93kliNgEcHaG8);
  background-repeat: no-repeat;
  background-size: cover;
  content: '';
  height: 100%;
  position: fixed;
  width: 100%;
  z-index: -1;
}

.container .container-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
}

@media (max-width: 1024px) {
  .container-wrapper {
    padding: 1.6rem;
  }

  h1,
  h3 {
    text-align: center;
  }

  h1 {
    font-size: 2rem;
  }

  h3 {
    font-size: 1rem;
  }

  p {
    font-size: 1rem;
  }

  .form-group input {
    font-size: 1.1rem;
  }

  .fieldset-form {
    width: 100%;
    flex-direction: column;
  }

  img {
    width: 100% !important;
    max-width: 300px;
  }

  .container-render {
    height: 500px !important;
  }
}
</style>
