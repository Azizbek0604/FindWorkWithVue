<script setup>
    import { reactive, defineProps, onMounted } from 'vue';
    import JonSingle from './JonSingle.vue';
    import { RouterLink } from 'vue-router';
    import PulseLoader from 'vue-spinner/src/PulseLoader.vue' 
    import axios from 'axios';

    defineProps({
        limit: {
            type: Number,
        },
        showButton: {
            type: Boolean,
            default: false
        }
    })

    const state = reactive({
        jobs: [],
        isLoading: true
    })

    onMounted(async () => {
    try {
        const res = await axios.get('http://localhost:8000/jobs');
        state.jobs = res.data;
    } catch (error) {
        console.error('Error fetching jobs', error);
    } finally {
        state.isLoading = false;
    }
});

</script>

<template>

    <section class="bg-blue-50 px-4 py-10">
        <div class="container-xl lg:container m-auto">
            <h2 class="text-3xl font-bold text-green-500 mb-6 text-center">Browse Jobs</h2>
            <!--Show Loading spinnerwhile loading is true-->
            <div v-if="state.isLoading" class="text-center text-gray-500 py-6">
                <PulseLoader/>
            </div>
            <!--Show Jobs listing when done Loading-->
            <div v-else class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <JonSingle v-for="job in state.jobs.slice(0, limit || state.jobs.lenght)" 
                :key="job.id" 
                :job="job"/>
            </div>
        </div>
    </section>

    <section v-if="showButton" class="m-auto max-w-lg my-10 px-6">
        <RouterLink
          to="/jobs"
          class="block bg-black text-white text-center py-4 px-6 rounded-xl hover:bg-gray-700"
          >View All Jobs</RouterLink
        >
      </section>
</template>