<script setup>
    import { ref, computed, onMounted, onUnmounted } from 'vue';
    import InputRange from './InputRange.vue';
    import InputText from './InputText.vue';
    import inputCrop from './inputCrop.vue';
    import SelectFormat from './SelectFormat.vue';

    const emit = defineEmits(['imageGenerated', 'showToast']);
    const isMobile = ref(false);
    const maxSize = computed(() => isMobile.value ? 3000 : 4000);
    
    const width = ref(1920);
    const height = ref(1080);
    const cropType = ref('none');
    const theme = ref('');
    const format = ref('webp');

    const checkMobile = () => {
        isMobile.value = window.innerWidth < 768;
        if (width.value > maxSize.value) width.value = maxSize.value;
        if (height.value > maxSize.value) height.value = maxSize.value;
    };

    onMounted(() => {
        checkMobile();
        window.addEventListener('resize', checkMobile);
    });

    onUnmounted(() => {
        window.removeEventListener('resize', checkMobile);
    });
    const isLoading = ref(false);
    const currentPage = ref(1);
    const lastSearchTerm = ref('');
    const originalImage = ref('');
    
    const getRandomPage = (total, current) => {
        if (total <= 1) return 1;
        let newPage;
        do {
            newPage = Math.floor(Math.random() * total) + 1;
        } while (newPage === current);
        return newPage;
    };

    const applyShape = async (imageUrl, shape) => {
        if (!shape || shape === 'none') return imageUrl;

        try {
            const maskParams = {
                circle: 'mask=ellipse',
                square: 'mask=corners'
            };

            const maskParam = maskParams[shape];
            if (maskParam) {
                return `${imageUrl}&${maskParam}`;
            }

            return imageUrl;
        } catch (error) {
            console.error('Erro ao aplicar shape:', error);
            return imageUrl;
        }
    };

    const generateImage = async () => {
        if (isLoading.value) return;
        
        isLoading.value = true;
        try {
            const API_URL = import.meta.env.VITE_UNSPLASH_API_URL;
            const ACCESS_KEY = import.meta.env.VITE_UNSPLASH_ACCESS_KEY;
            const searchTerm = theme.value.trim() || 'natureza';

            if (searchTerm === lastSearchTerm.value) {
                const totalPages = 20;
                currentPage.value = getRandomPage(totalPages, currentPage.value);
            } else {
                currentPage.value = 1;
                lastSearchTerm.value = searchTerm;
            }
            
            const endpoint = `${API_URL}/search/photos?query=${encodeURIComponent(searchTerm)}&per_page=1&page=${currentPage.value}`;
            
            const response = await fetch(endpoint, {
                headers: {
                    Authorization: `Client-ID ${ACCESS_KEY}`
                }
            });

            if (!response.ok) throw new Error('Erro ao buscar imagem');
            
            const data = await response.json();
            
            if (data.results.length === 0) {
                throw new Error('Nenhuma imagem encontrada para esse tema');
            }

            let transformParams = `w=${width.value}&h=${height.value}&fit=crop&fm=${format.value}`;
            const baseImageUrl = `${data.results[0].urls.raw}&${transformParams}`;
            
            originalImage.value = baseImageUrl;
            
            const finalImageUrl = await applyShape(baseImageUrl, cropType.value);
            emit('imageGenerated', finalImageUrl, format.value);
            
        } catch (error) {
            console.error('Erro na requisição:', error);
        } finally {
            isLoading.value = false;
        }
    };
</script>
<template>
    <form class="w-full flex flex-col gap-4" @submit.prevent="generateImage">
        <InputText 
            v-model="theme"
            label="Procure por tema" 
            placeholder="Ex: carro, floresta, cidade..." 
        />
        <InputRange 
            v-model="width" 
            label="Largura:" 
            :min="0" 
            :max="maxSize" 
        />
        <InputRange 
            v-model="height" 
            label="Altura:" 
            :min="0" 
            :max="maxSize" 
        />
        <inputCrop 
            class="hidden lg:block"
            v-model="cropType" 
            label="Tipo de Shape" 
        />
        <SelectFormat
            v-model="format"
            label="Formato da imagem"
        />
        <button
            type="submit"
            class="btn btn-info text-white font-semibold mt-4 relative min-h-10 w-full"
            :class="{ 'loading before:absolute! before:left-1/2! before:-translate-x-1/2 before:w-4 before:h-4': isLoading }"
            :disabled="isLoading"
        >
            <span :class="{ 'opacity-0': isLoading }">
                {{ isLoading ? 'Gerando...' : 'Gerar imagem' }}
            </span>
        </button>
    </form>
</template>

<script>

</script>
