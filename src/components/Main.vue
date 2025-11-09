<script setup>
import Header from '../components/Header/Header.vue';
import Body from '../components/Body/Body.vue';
</script>

<template>
    <div class="h-screen bg-base-100 flex flex-col overflow-hidden md:w-auto w-full">
        <Header />
        <div class="flex-1 container mx-auto md:min-w-md lg:min-w-[1440px] max-w-[1440px] px-4 overflow-hidden">
            <Body />
        </div>
    </div>
</template>

