<script setup lang="ts">
import { onMounted, reactive } from 'vue';
import axios from 'axios';
import { VueSpinnerPulse } from 'vue3-spinners';
import JobListing from '@/components/JobListing.vue';
import type { JobValue } from '@/types';

const { showButton = false } = defineProps<{ limit?: number; showButton?: boolean }>();

interface State {
  jobs: JobValue[];
  isLoading: boolean;
}
const state = reactive<State>({
  jobs: [],
  isLoading: true,
});

onMounted(async () => {
  try {
    const response = await axios.get('/api/jobs');
    state.jobs = response.data;
  } catch (err) {
    console.error('error fetching jobs: ', err);
  } finally {
    state.isLoading = false;
  }
});
</script>

<template>
  <section class="bg-blue-50 px-4 py-10 flex-1">
    <div class="container mx-auto">
      <h2 class="text-3xl font-bold text-green-600 mb-6 text-center">Browse Jobs</h2>
      <div v-if="state.isLoading" class="text-center text-gray-500 py-6">
        <VueSpinnerPulse color="#6a7282" />
      </div>
      <div v-else class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <JobListing
          v-for="job in state.jobs.slice(0, limit || state.jobs.length)"
          :key="job.id"
          :job="job"
        />
      </div>
    </div>
    <section v-if="showButton" class="mx-auto max-w-lg my-10 px-6">
      <RouterLink
        to="/jobs"
        class="block bg-black text-white text-center py-4 px-6 rounded-xl hover:bg-gray-700"
      >
        View All Jobs
      </RouterLink>
    </section>
  </section>
</template>
