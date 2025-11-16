<script setup lang="ts">
import logo from '@/assets/img/logo.png';
import { useRoute } from 'vue-router';

const isActiveLink = (routePath: string) => {
  const route = useRoute();
  return route.path === routePath;
};
</script>

<template>
  <nav class="bg-green-700 border-b border-green-500">
    <div class="mx-auto max-w-7xl px-2 sm:px-6 lg:px-8">
      <div class="flex h-20 items-center">
        <div class="flex flex-1 items-center justify-center">
          <RouterLink class="flex items-center mr-2" to="/">
            <img class="h-10 w-auto" :src="logo" alt="" />
            <span class="hidden md:block text-white text-2xl font-bold ml-2">Vue Jobs</span>
          </RouterLink>
          <div class="md:ml-auto">
            <div class="flex space-x-2">
              <RouterLink
                to="/"
                :class="[
                  isActiveLink('/') ? 'bg-green-900 hover:bg-gray-900' : 'hover:bg-green-900',
                  'text-white',
                  'rounded-md',
                  'px-3',
                  'py-2',
                ]"
              >
                Home
              </RouterLink>
              <RouterLink
                to="/jobs"
                :class="[
                  isActiveLink('/jobs') ? 'bg-green-900 hover:bg-gray-900' : 'hover:bg-green-900',
                  'text-white',
                  'rounded-md',
                  'px-3',
                  'py-2',
                ]"
              >
                Jobs
              </RouterLink>
              <RouterLink
                to="/jobs/add"
                :class="[
                  isActiveLink('/jobs/add')
                    ? 'bg-green-900 hover:bg-gray-900'
                    : 'hover:bg-green-900',
                  'text-white',
                  'rounded-md',
                  'px-3',
                  'py-2',
                ]"
              >
                Add Job
              </RouterLink>
            </div>
          </div>
        </div>
      </div>
    </div>
  </nav>
</template>
