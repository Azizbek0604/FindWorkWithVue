<script setup>
import Hero from '@/comonents/Hero.vue'; 
import HomeCards from '@/comonents/HomeCards.vue'; 
import JobListings from '@/comonents/JobListings.vue'; 
</script>

<template>
    <Hero />
    <HomeCards />
    <JobListings :limit="3" :showButton="true" />
</template>
