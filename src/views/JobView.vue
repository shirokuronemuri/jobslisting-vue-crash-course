<script setup lang="ts">
import { onMounted, reactive } from 'vue';
import { useRoute } from 'vue-router';
import axios, { AxiosError } from 'axios';
import { VueSpinnerPulse } from 'vue3-spinners';
import type { JobValue } from '@/types';
import BackButton from '@/components/BackButton.vue';
import router from '@/router';
import { useToast } from 'vue-toastification';
import NotFoundView from './NotFoundView.vue';

const jobId = useRoute().params.id;

interface State {
  job: JobValue | null;
  isLoading: boolean;
  isNotFound: boolean;
}
const state = reactive<State>({
  job: null,
  isLoading: true,
  isNotFound: false,
});

onMounted(async () => {
  try {
    const response = await axios.get(`/api/jobs/${jobId}`);
    state.job = response.data;
  } catch (err) {
    if (err instanceof AxiosError && err.response && err.response.status === 404) {
      state.isNotFound = true;
    }
    console.error('error fetching jobs: ', err);
  } finally {
    state.isLoading = false;
  }
});

const toast = useToast();

const deleteJob = async () => {
  try {
    const confirm = window.confirm('Are you sure you want to delete the job?');
    if (confirm) {
      await axios.delete(`/api/jobs/${jobId}`);
      toast.success('Job deleted successfully!');
      router.push('/jobs');
    }
  } catch (err) {
    toast.error('Error deleting the job');
    console.error(err);
  }
};
</script>

<template>
  <div
    v-if="state.isLoading"
    class="text-center text-gray-500 flex flex-1 justify-center items-center py-6"
  >
    <VueSpinnerPulse color="#6a7282" class="flex-1" />
  </div>
  <NotFoundView v-else-if="state.isNotFound" />
  <div v-if="state.job">
    <BackButton />

    <section class="bg-green-50">
      <div class="container mx-auto py-10 px-6">
        <div class="grid grid-cols-1 lg:grid-cols-70/30 w-full gap-6">
          <main>
            <div class="bg-white p-6 rounded-lg shadow-md text-center md:text-left">
              <div class="text-gray-500 mb-4">{{ state.job.type }}</div>
              <h1 class="text-3xl font-bold mb-4">{{ state.job.title }}</h1>
              <div class="text-gray-500 mb-4 flex align-middle justify-center md:justify-start">
                <i class="pi pi-map-marker text-orange-700 mr-2 text-lg"></i>
                <p class="text-orange-700">{{ state.job.location }}</p>
              </div>
            </div>
            <div class="bg-white p-6 rounded-lg shadow-md mt-6">
              <h3 class="text-green-800 text-lg font-bold mb-6">Job Description</h3>
              <p class="mb-4">{{ state.job.description }}</p>
              <h3 class="text-green-800 text-lg font-bold mb-2">Salary</h3>
              <p class="mb-4">{{ state.job.salary }} / Year</p>
            </div>
          </main>

          <aside>
            <div class="bg-white p-6 rounded-lg shadow-md">
              <h3 class="text-xl font-bold mb-6">Company Info</h3>
              <h2 class="text-2xl">{{ state.job.company.name }}</h2>
              <p class="my-2">{{ state.job.company.description }}</p>
              <hr class="my-4" />
              <h3 class="text-xl">Contact Email:</h3>
              <p class="my-2 bg-green-100 p-2 font-bold">{{ state.job.company.contactEmail }}</p>
              <h3 class="text-xl">Contact Phone:</h3>
              <p class="my-2 bg-green-100 p-2 font-bold">{{ state.job.company.contactPhone }}</p>
            </div>
            <!-- Manage -->
            <div class="bg-white p-6 rounded-lg shadow-md mt-6">
              <h3 class="text-xl font-bold mb-6">Manage Job</h3>
              <RouterLink
                :to="`/jobs/edit/${state.job.id}`"
                class="bg-green-500 hover:bg-green-600 text-white text-center font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline mt-4 block"
              >
                Edit Job
              </RouterLink>
              <button
                @click="deleteJob"
                class="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline mt-4 block"
              >
                Delete Job
              </button>
            </div>
          </aside>
        </div>
      </div>
    </section>
  </div>
</template>
