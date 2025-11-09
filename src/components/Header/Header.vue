<script setup>
    import HeaderLogo from './HeaderLogo.vue';
    import HeaderButtonTheme from './HeaderButtonTheme.vue';
    import HeaderProfile from './HeaderProfile.vue';
</script>
<template>
   <header class="bg-base-100 border-b border-base-200">
        <nav class="flex justify-between items-center max-w-[1440px] mx-auto px-4 py-3">
            <HeaderLogo />
            <div class="flex gap-4 items-center">
                <HeaderButtonTheme />
                <HeaderProfile />
            </div>
        </nav>
   </header>
</template>