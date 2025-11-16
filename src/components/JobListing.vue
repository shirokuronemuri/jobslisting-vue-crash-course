<script setup lang="ts">
import type { JobValue } from '@/types';
import { computed, ref } from 'vue';

const { job } = defineProps<{ job: JobValue }>();

const showFullDescription = ref(false);

const toggleFullDescription = () => {
  showFullDescription.value = !showFullDescription.value;
};

const needsTruncatedDescription = job.description.length > 90;

const truncatedDescription = computed(() => {
  let description = job.description;
  if (!showFullDescription.value && needsTruncatedDescription) {
    description = description.substring(0, 90) + '...';
  }
  return description;
});
</script>

<template>
  <div class="bg-white rounded-xl shadow-md p-4 flex flex-col">
    <div class="flex-1">
      <div class="mb-6">
        <div class="text-gray-600 my-2">{{ job.type }}</div>
        <h3 class="text-xl font-bold">{{ job.title }}</h3>
      </div>

      <div class="mb-5">
        <div>
          {{ truncatedDescription }}
        </div>
        <button
          v-if="needsTruncatedDescription"
          class="text-green-500 hover:text-green-600"
          @click="toggleFullDescription"
        >
          {{ showFullDescription ? 'Show less' : 'Show more' }}
        </button>
      </div>

      <h3 class="text-green-500 mb-2">{{ job.salary }} / Year</h3>
    </div>
    <div>
      <div class="border border-gray-100 mb-3"></div>

      <div class="flex flex-col lg:flex-row lg:items-baseline justify-between">
        <div class="text-orange-700 mb-3">
          <i class="pi pi-map-marker text-orange-700"></i>
          {{ job.location }}
        </div>
        <RouterLink
          :to="'/jobs/' + job.id"
          class="h-9 bg-green-500 hover:bg-green-600 text-white px-4 py-2 rounded-lg text-center text-sm"
        >
          Read More
        </RouterLink>
      </div>
    </div>
  </div>
</template>
