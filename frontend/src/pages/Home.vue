<template>
  <div>
    <header
      class="sticky top-0 z-10 flex items-center justify-between border-b bg-white px-5 py-2.5"
    >
      <PageBreadcrumbs class="h-7" :items="breadcrumbs" />
    </header>

    <router-view v-slot="{ Component, route }">
      <div :class="{ 'mx-auto w-full max-w-4xl px-5': !route.meta?.fullWidth }">
        <component :is="Component" />
      </div>
    </router-view>
  </div>
</template>
<script setup>
import { computed } from 'vue'
import { useRoute } from 'vue-router'
import { getCachedDocumentResource, usePageMeta } from 'frappe-ui'

let breadcrumbs = computed(() => {
  let route = useRoute()
  let items = [{ label: 'Home', route: { name: 'Home' } }]
  if (route.name === 'HomeDiscussions') {
    items.push({
      label: 'Discussions',
      route: { name: 'HomeDiscussions' },
    })
  }
  if (['HomeTasks', 'HomeTask'].includes(route.name)) {
    items.push({
      label: 'My Tasks',
      route: { name: 'HomeTasks' },
    })
  }
  if (route.name === 'HomeTask') {
    let task = getCachedDocumentResource('GP Task', route.params.taskId)
    items.push({
      label: task?.doc ? task.doc.title : route.params.taskId,
      route: { name: 'HomeTask' },
    })
  }
  return items
})

usePageMeta(() => {
  return {
    title: 'Home',
  }
})
</script>
