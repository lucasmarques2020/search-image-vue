<script setup>
defineProps({
  image: {
    type: String,
    required: true
  }
})
</script>
<template>
  <div class="flex flex--center-x flex--center-y container-render">
    <div
      class="flex flex--center-x flex--center-y renger-image"
      :class="{ 'container-render renger-image copied': copied }"
      @click="setCopyElement()"
    >
      <p v-if="image == ''" class="text-white">Sua imagem será gerada aqui!</p>
      <img v-else :id="idRenderImage" :src="image" alt="Imagem renderizada" />
    </div>
  </div>
  <textarea v-if="textArea" :value="valueTextArea"></textarea>
</template>
<script>
export default {
  data() {
    return {
      renderImageClass: 'flex flex--center-x flex--center-y renger-image',
      idRenderImage: 'id-renger-image',
      copied: false,
      copiedAgain: false,
      textArea: false,
      valueTextArea: ''
    }
  },
  methods: {
    setCopyElement() {
      if (this.copied) {
        this.copiedAgain = true
      }

      this.copyElement(this.image)
      this.copied = true
    },

    copyElement(textCopy) {
      this.textArea = textCopy != '' ? true : false
      if (this.textArea) {
        this.copied = true
        this.valueTextArea = textCopy
        navigator.clipboard.writeText(textCopy)
        this.textArea = false
      }
    }
  }
}
</script>
<style scoped>
.container-render {
  background: #424242;
  border-radius: 10px;
  height: 600px;
  margin: 50px auto;
  max-width: 750px;
  overflow: auto;
  padding: 20px;
  width: 100%;
}

.container-render .renger-image {
  cursor: pointer;
  background-color: #313131;
  border-radius: 8px;
  height: 100%;
  padding: 3rem;
  position: relative;
  width: 100%;
}

.container-render .renger-image:hover::before {
  content: 'Clique para copiar';
  background-color: rgba(0, 0, 0, 0.555);
  color: white;

  display: flex;
  justify-content: center;
  align-items: center;

  width: 100%;
  height: 100%;
  border-radius: 8px;

  position: absolute;
}

.container-render .renger-image.copied:hover {
  cursor: pointer;
}

.container-render .renger-image.copied:hover::before {
  content: 'Copiado!';
  background-color: #026b226c;
}

.container-render .renger-image.copied-again:hover {
  cursor: pointer;
}

.container-render .renger-image.copied-again:hover::before {
  content: 'Copiado novamente!';
  background-color: #098d316c;
}

img {
  width: 580px;
  height: 100%;
  object-fit: cover;
}

@media (max-width: 768px) {
  #id-renger-image {
    width: 280px !important;
  }
}
</style>
