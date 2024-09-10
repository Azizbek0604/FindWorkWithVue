<script setup>
import PulseLoader from 'vue-spinner/src/PulseLoader.vue';
import axios from 'axios';
import { reactive, onMounted } from 'vue';
import { useRoute, RouterLink, useRouter } from 'vue-router';
import BackButton from '@/comonents/BackButton.vue';
import { useToast } from 'vue-toastification';


const route = useRoute();
const router = useRouter();
const toast = useToast();


const jobId = route.params.id

const state = reactive ({
    jobs: [],
    isLoading: true
})

const delateJob = async () => {
  try {
    const confirm = window.confirm('Are you sure you want to delete this job? ')
    if (confirm) {
    await axios.delete(`http://localhost:8000/jobs/${jobId}`)
    toast.success('Job Deleted Successfully')
    router.push('/jobs')
    }
  } catch (error) {
    console.error('Error delating job', error);
    toast.error('Job Not Deleted')
  }
}

onMounted(async () => {
    try {
        const res = await axios.get(`http://localhost:8000/jobs/${jobId}`);
        state.jobs = res.data;
    } catch (error) {
        console.error('Error fetching job', error);
    } finally {
        state.isLoading = false;
    }
});

</script>

<template>
    <BackButton/>
    <section v-if="!state.isLoading" class="bg-green-50">
        <div class="container m-auto py-10 px-6">
          <div class="grid grid-cols-1 md:grid-cols-70/30 w-full gap-6">
            <main>
              <div
                class="bg-white p-6 rounded-lg shadow-md text-center md:text-left"
              >
                <div class="text-gray-500 mb-4">{{state.jobs.type}}</div>
                <h1 class="text-3xl font-bold mb-4">{{state.jobs.title}}</h1>
                <div
                  class="text-gray-500 mb-4 flex align-middle justify-center md:justify-start"
                >
                  <i
                    class="pi pi-map-marker text-xl text-orange-700 mr-2"
                  ></i>
                  <p class="text-orange-700">{{state.jobs.location}}</p>
                </div>
              </div>
  
              <div class="bg-white p-6 rounded-lg shadow-md mt-6">
                <h3 class="text-green-800 text-lg font-bold mb-6">
                  Job Description
                </h3>
  
                <p class="mb-4">
                    {{state.jobs.description}}
                </p>
  
                <h3 class="text-green-800 text-lg font-bold mb-2">Salary</h3>
  
                <p class="mb-4">{{state.jobs.salary}} / Year</p>
              </div>
            </main>
  

            <aside>

              <div class="bg-white p-6 rounded-lg shadow-md">
                <h3 class="text-xl font-bold mb-6">Company Info</h3>
  
                <h2 class="text-2xl">{{state.jobs.company.name}}</h2>
  
                <p class="my-2">
                   {{ state.jobs.company.description}}
                </p>
  
                <hr class="my-4" />
  
                <h3 class="text-xl">Contact Email:</h3>
  
                <p class="my-2 bg-green-100 p-2 font-bold">
                    {{ state.jobs.company.contactEmail}}
                </p>
  
                <h3 class="text-xl">Contact Phone:</h3>
  
                <p class="my-2 bg-green-100 p-2 font-bold">{{ state.jobs.company.contactPhone}}</p>
              </div>
  
              <div class="bg-white p-6 rounded-lg shadow-md mt-6">
                <h3 class="text-xl font-bold mb-6">Manage Job</h3>
                <RouterLink
                  :to="`/jobs/edit${state.jobs.id}`"
                  class="bg-green-500 hover:bg-green-600 text-white text-center font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline mt-4 block"
                  >Edit Job
                </RouterLink>
                <button @click="delateJob"
                  class="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline mt-4 block"
                >
                  Delete Job
                </button>
              </div>
            </aside>
          </div>
        </div>
      </section>

      <div v-else class="text-center text-gray-500 py-6">
        <PulseLoader/>
    </div>
</template>