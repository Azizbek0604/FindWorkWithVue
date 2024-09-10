<script setup>
import Navbar from '@/comonents/Navbar.vue'; // Ensure the path is correct
import { RouterView } from 'vue-router';
</script>

<template>
    <Navbar />
    <RouterView />
</template>