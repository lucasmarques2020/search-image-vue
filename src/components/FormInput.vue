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
      <button :id="btnForm" :class="{ loading: isLoading }" type="button" @click="render()">
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
      btnForm: '',
      isLoading: false
    }
  },
  methods: {
    render() {
      const width = this.formWidth
      const height = this.formHeight

      const API_URL = `https://picsum.photos/${width}/${height}.webp?random`

      this.isLoading = true
      fetch(API_URL)
        .then((response) => {
          if (!response.ok) {
            throw new Error('Erro ao buscar a imagem')
          }
          return response
        })
        .then((response) => {
          const imageUrl = response.url
          this.setImageContainer(imageUrl)
        })
        .catch((error) => {
          console.error('Erro na requisição:', error)
        })
        .finally(() => {
          this.isLoading = false
        })
    },

    setImageContainer(image) {
      this.$emit('renderImage', image)
    },

    inputRules(event) {
      const input = event.target
      let value = parseInt(input.value)

      const min = parseInt(input.min)
      const max = parseInt(input.max)

      if (value < min) {
        value = min
      } else if (value > max) {
        value = max
      }

      if (input.id === 'id-width') {
        this.formWidth = value
      } else if (input.id === 'id-height') {
        this.formHeight = value
      }

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
