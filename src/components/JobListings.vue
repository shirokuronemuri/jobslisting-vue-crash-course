<script setup lang="ts">
import jobData from '@/jobs.json';
import { ref } from 'vue';
import JobListing from '@/components/JobListing.vue';

const { showButton = false } = defineProps<{ limit: number; showButton: boolean }>();

const jobs = ref(jobData);
</script>

<template>
  <section class="bg-blue-50 px-4 py-10">
    <div class="container mx-auto">
      <h2 class="text-3xl font-bold text-green-600 mb-6 text-center">Browse Jobs</h2>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <JobListing v-for="job in jobs.slice(0, limit || jobs.length)" :key="job.id" :job="job" />
      </div>
    </div>
    <section v-if="showButton" class="mx-auto max-w-lg my-10 px-6">
      <a
        href="/jobs"
        class="block bg-black text-white text-center py-4 px-6 rounded-xl hover:bg-gray-700"
      >
        View All Jobs
      </a>
    </section>
  </section>
</template>
