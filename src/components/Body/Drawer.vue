<script setup>
    import { ref } from 'vue';
    import Form from '../Form/Form.vue';
    import RenderImage from '../RenderImage/RenderImage.vue';

    const currentImage = ref('');
    const format = ref('webp');
    const toast = ref({ show: false, message: '', type: '' });

    const handleImageGenerated = (url, selectedFormat) => {
        currentImage.value = url;
        format.value = selectedFormat;
    };

    const showToast = ({ message, type = 'info' }) => {
        toast.value = {
            show: true,
            message,
            type: `alert-${type}`
        };
        
        setTimeout(() => {
            toast.value.show = false;
        }, 3000);
    };
</script>

<template>
    <div>
        <div class="drawer drawer-open lg:drawer-mobile">
            <input id="my-drawer-4" type="checkbox" class="drawer-toggle" checked />
            <div class="drawer-content flex flex-col lg:flex-row bg-base-100">
                <div class="lg:hidden w-full bg-base-200 p-4">
                    <Form 
                        @imageGenerated="handleImageGenerated" 
                        @showToast="showToast" 
                    />
                </div>
                <RenderImage 
                    :imageUrl="currentImage"
                    :format="format" 
                    @showToast="showToast" 
                />
            </div>
            <div class="drawer-side hidden lg:block">
                <label for="my-drawer-4" aria-label="close sidebar" class="drawer-overlay"></label>
                <div class="w-80 bg-base-200 min-h-full">
                    <div class="w-full p-4">
                        <Form 
                            @imageGenerated="handleImageGenerated" 
                            @showToast="showToast" 
                        />
                    </div>
                </div>
            </div>
        </div>
        
        <div class="toast toast-end z-50">
            <div v-if="toast.show" :class="['alert', toast.type]">
                {{ toast.message }}
            </div>
        </div>
    </div>
</template>