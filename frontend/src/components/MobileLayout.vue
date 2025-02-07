<template>
  <div class="flex h-full flex-col">
    <div class="h-full overflow-auto" id="scrollContainer">
      <slot />
    </div>
    <div
      class="grid grid-cols-5 border-t border-gray-300 standalone:pb-4"
      :style="{ gridTemplateColumns: `repeat(${tabs.length}, minmax(0, 1fr))` }"
    >
      <button
        v-for="tab in tabs"
        :key="tab.name"
        class="flex flex-col items-center justify-center py-3 transition active:scale-95"
        @click="onTabClick(tab)"
      >
        <component
          :is="tab.icon"
          class="h-6 w-6"
          :class="[tab.isActive ? 'text-gray-900' : 'text-gray-600']"
        />
      </button>
    </div>
  </div>
</template>
<script>
import { scrollTo } from '@/utils/scrollContainer'
import LucideHome from '~icons/lucide/home'
import LucideUsers2 from '~icons/lucide/users-2'
import LucideSearch from '~icons/lucide/search'
import LucideInbox from '~icons/lucide/inbox'
import LucideLayoutGrid from '~icons/lucide/layout-grid'

export default {
  name: 'MobileLayout',
  computed: {
    tabs() {
      return [
        {
          name: 'Home',
          icon: LucideHome,
          route: { name: 'Home' },
          isActive: [
            'Home',
            'HomeOverview',
            'HomeDiscussions',
            'HomeTasks',
            'HomeTask',
          ].includes(this.$route.name),
        },
        {
          name: 'Teams',
          icon: LucideLayoutGrid,
          route: { name: 'Teams' },
          isActive: [
            'Teams',
            'TeamOverview',
            'ProjectOverview',
            'ProjectDiscussions',
            'ProjectDiscussion',
            'ProjectDiscussionNew',
            'ProjectTasks',
            'ProjectTaskDetail',
          ].includes(this.$route.name),
        },
        {
          name: 'People',
          icon: LucideUsers2,
          route: { name: 'People' },
          isActive: /People|PersonProfile/g.test(this.$route.name),
          condition: () => this.$user().isNotGuest,
        },
        {
          name: 'Search',
          icon: LucideSearch,
          route: { name: 'Search' },
          isActive: this.$route.name === 'Search',
          condition: () => this.$user().isNotGuest,
        },
        {
          name: 'Notifications',
          icon: LucideInbox,
          route: { name: 'Notifications' },
          isActive: this.$route.name === 'Notifications',
        },
      ].filter((tab) => (tab.condition ? tab.condition() : true))
    },
  },
  methods: {
    onTabClick(tab) {
      if (tab.isActive) {
        scrollTo({ top: 0, behavior: 'smooth' })
        return
      }
      this.$router.push(tab.route)
    },
  },
}
</script>
