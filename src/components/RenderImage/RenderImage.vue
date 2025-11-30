<script setup>
import { ref } from 'vue';

const props = defineProps({
  imageUrl: {
    type: String,
    default: ''
  },
  format: {
    type: String,
    default: 'webp'
  },
  imageLoading: {
    type: Boolean,
    default: false
  }
});

const emit = defineEmits(['showToast', 'imageLoaded']);
const copied = ref(false);
const idRenderImage = ref('id-render-image');

const copyImageUrl = (url) => {
    if (!url) return;
    
    navigator.clipboard.writeText(url)
        .then(() => {
            copied.value = true;
            emit('showToast', {
                message: 'URL copiada com sucesso!',
                type: 'success'
            });
            setTimeout(() => {
                copied.value = false;
            }, 2000);
        })
        .catch(err => {
            console.error('Erro ao copiar URL:', err);
            emit('showToast', {
                message: 'Erro ao copiar URL',
                type: 'error'
            });
        });
};

const onImgLoad = () => {
  emit('imageLoaded');
};

const onImgError = (err) => {
  console.error('Erro ao carregar imagem:', err);
  emit('showToast', { message: 'Erro ao carregar imagem', type: 'error' });
  emit('imageLoaded');
};

const downloadImage = async () => {
    if (!props.imageUrl) return;
    
    try {
        const response = await fetch(props.imageUrl);
        const blob = await response.blob();
        const url = window.URL.createObjectURL(blob);
        const a = document.createElement('a');
        a.href = url;
        a.download = `imagem-gerada.${props.format}`;
        document.body.appendChild(a);
        a.click();
        document.body.removeChild(a);
        window.URL.revokeObjectURL(url);
        
        emit('showToast', {
            message: 'Download iniciado!',
            type: 'success'
        });
    } catch (error) {
        console.error('Erro ao fazer download:', error);
        emit('showToast', {
            message: 'Erro ao fazer download',
            type: 'error'
        });
    }
};
</script>
<template>
  <div class="h-auto max-h-[48rem] w-full p-4 pb-0">
    <div class="bg-base-300 rounded-lg h-full w-full flex items-center justify-center relative overflow-hidden">
      <div class="w-full h-full flex flex-col items-center justify-center p-2 sm:p-4 md:p-6 relative">
        <p v-if="!imageUrl" class="text-base-content text-lg">
          Sua imagem será gerada aqui!
        </p>
        
        <template v-else>
            <div class="relative group bg-white p-2 sm:p-4 rounded-lg w-full h-full flex items-center justify-center overflow-hidden">
            <img 
              :id="idRenderImage" 
              :src="imageUrl" 
              alt="Imagem renderizada" 
              @load="onImgLoad"
              @error="onImgError"
              class="w-auto h-auto max-w-[calc(100vw-2rem)] md:max-w-[calc(100vw-4rem)] max-h-[calc(100vh-10rem)] md:max-h-[calc(100vh-12rem)] object-contain"
            />

            <div v-if="props.imageLoading" class="absolute inset-0 bg-base-100/75 flex items-center justify-center z-20">
              <div class="loading loading-spinner loading-lg"></div>
            </div>
            <div 
              class="absolute inset-0 bg-base-100/75 flex items-center h-full max-h-full justify-center opacity-0 group-hover:opacity-100 transition-opacity duration-200"
            >
              <div class="flex gap-4">
                <button
                  @click="copyImageUrl(imageUrl)"
                  class="btn bg-green-500 hover:bg-green-600 text-white border-0"
                >
                  <svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 5H6a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2v-1M8 5a2 2 0 002 2h2a2 2 0 002-2M8 5a2 2 0 012-2h2a2 2 0 012 2m0 0h2a2 2 0 012 2v3m2 4H10m0 0l3-3m-3 3l3 3"></path>
                  </svg>
                  {{ copied ? 'Copiado!' : 'Copiar URL' }}
                </button>
                <button
                  @click="downloadImage"
                  class="btn btn-info text-white"
                >
                  <svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4"></path>
                  </svg>
                  Download
                </button>
              </div>
            </div>
          </div>
        </template>
      </div>
    </div>
  </div>
</template>
<script>

</script>

