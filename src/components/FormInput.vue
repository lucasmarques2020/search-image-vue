<template>
  <div class="container-form">
    <form id="form-send-information" class="form-group flex__direction--column">
      <div class="flex fieldset-form">
        <fieldset>
          <input
            type="number"
            v-model.number="formWidth"
            @input="inputRules"
            placeholder="1360"
            min="0"
            max="5000"
            id="id-width"
          />
          <label for="id-width">(px)</label>
        </fieldset>

        <fieldset>
          <input
            type="number"
            v-model.number="formHeight"
            @input="inputRules"
            placeholder="768"
            min="0"
            max="5000"
            id="id-height"
          />
          <label for="id-height">(px)</label>
        </fieldset>
      </div>

      <fieldset>
        <input
          type="text"
          v-model="formTheme"
          placeholder="Ex: arvore, carro, cidade..."
          id="id-theme"
        />
      </fieldset>

      <button
        :id="btnForm"
        :class="{ loading: isLoading }"
        type="button"
        @click="render()"
      >
        {{ isLoading ? '' : 'Gerar' }}
      </button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'form-send-information',
  data() {
    return {
      formWidth: 1360,
      formHeight: 768,
      formTheme: 'natureza',
      btnForm: '',
      isLoading: false
    }
  },
  methods: {
    async render() {
      const width = this.formWidth
      const height = this.formHeight
      const theme = this.formTheme.trim() || 'natureza'
      const API_URL = import.meta.env.VITE_UNSPLASH_API_URL
      const ACCESS_KEY = import.meta.env.VITE_UNSPLASH_ACCESS_KEY

      const endpoint = `${API_URL}/search/photos?query=${encodeURIComponent(theme)}&per_page=1`

      this.isLoading = true

      try {
        const response = await fetch(endpoint, {
          headers: {
            Authorization: `Client-ID ${ACCESS_KEY}`
          }
        })

        if (!response.ok) throw new Error('Erro ao buscar imagem')

        const data = await response.json()

        if (data.results.length === 0) {
          throw new Error('Nenhuma imagem encontrada para esse tema')
        }

        const imageUrl = `${data.results[0].urls.raw}&w=${width}&h=${height}&fit=crop&fm=webp`

        this.setImageContainer(imageUrl)
      } catch (error) {
        console.error('Erro na requisição:', error)
      } finally {
        this.isLoading = false
      }
    },

    setImageContainer(image) {
      this.$emit('renderImage', image)
    },

    inputRules(event) {
      const input = event.target
      let value = parseInt(input.value)
      const min = parseInt(input.min)
      const max = parseInt(input.max)

      if (value < min) value = min
      else if (value > max) value = max

      if (input.id === 'id-width') this.formWidth = value
      else if (input.id === 'id-height') this.formHeight = value

      input.value = value
    }
  }
}
</script>

<style>
.container-form {
  background: #424242;
  border-radius: 10px;
  margin: 50px auto;
  max-width: 750px;
  padding: 20px;
  width: 100%;
}

.container-form form {
  align-items: center;
  display: flex;
  gap: 10px;
  justify-content: center;
  flex-wrap: wrap;
}

.container-form form .fieldset-form {
  width: 100%;
}

.container-form form div {
  gap: 10px;
}

.container-form form fieldset {
  border: none;
  margin: 0;
  padding: 0;
  position: relative;
  width: 100%;
}

.container-form form input {
  border: none;
  border-radius: 8px;
  font-size: 1.25rem;
  height: 50px;
  padding: 0 20px;
  width: 100%;
}

.container-form form label {
  color: #333;
  font-size: 14px;
  font-weight: 400;
  left: 3.8rem;
  margin-left: 5px;
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
}

.container-form form button {
  background-color: #00a8c5;
  border: none;
  border-radius: 8px;
  color: #fff;
  cursor: pointer;
  font-size: 18px;
  font-weight: 700;
  height: 50px;
  transition: 0.2s ease-in-out;
  width: 100%;
}

@media (max-width: 768px) {
  #form-send-information {
    display: flex;
    flex-direction: column;
  }
}
</style>
