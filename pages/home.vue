<template>
  <NuxtLayout name="app">
    <view-wrapper class="space-y-12 py-12">
      <div v-if="projectStore.isReady && projectStore.projects.length > 0" class="w-full max-w-5xl mx-auto">
        <div class="flex items-start justify-center gap-6 flex-wrap">
          <UCard v-for="project in projectStore.projects" :key="'home-project-selection-' + project.id" class="w-[300px] h-[150px]">
            {{ project.name }}
          </UCard>
        </div>
      </div>
      <div v-else-if="!isLoading && projectStore.isReady">
        <div class="w-full max-w-screen-sm mx-auto block">
          <div class="flex items-center justify-center gap-2">
            <UCard class="w-full">
              <template #header>
                No projects found!
              </template>
              <p class="text-sm">
                You don't have any projects yet. Create a project to get started.
              </p>
              <template #footer>
                <UButton to="/project/create" label="Create Project" size="xl" />
              </template>
            </UCard>
          </div>
        </div>
      </div>
    </view-wrapper>
  </NuxtLayout>
</template>

<script setup>
import { useProjectStore } from '@/store/project'
definePageMeta({
  middleware: 'auth'
})

const projectStore = useProjectStore()
const isLoading = ref(false)

onMounted(() => {
  if (!projectStore.isReady) {
    isLoading.value = true
  }

  projectStore.fetchProjects()
    .finally(() => {
      isLoading.value = false
    })
})
</script>
