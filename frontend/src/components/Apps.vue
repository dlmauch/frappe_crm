<template>
  <Popover placement="right-start" class="flex w-full">
    <template #target="{ togglePopover }">
      <button
        :class="[
          'group w-full flex h-7 items-center justify-between rounded px-2 text-base text-gray-800 hover:bg-gray-100',
        ]"
        @click.prevent="togglePopover()"
      >
        <div class="flex gap-2">
          <AppsIcon class="size-4" />
          <span class="whitespace-nowrap">
            {{ __('Apps') }}
          </span>
        </div>
        <FeatherIcon name="chevron-right" class="size-4 text-gray-600" />
      </button>
    </template>
    <template #body>
      <div
        class="grid grid-cols-3 justify-between mx-3 p-2 rounded-lg border border-gray-100 bg-white shadow-xl"
      >
        <div v-for="app in apps.data" key="name">
          <a
            :href="app.route"
            class="flex flex-col gap-1.5 rounded justify-center items-center py-2 px-3 hover:bg-gray-100"
          >
            <img class="size-8" :src="app.logo" />
            <div class="text-sm" @click="app.onClick">
              {{ app.title }}
            </div>
          </a>
        </div>
      </div>
    </template>
  </Popover>
</template>
<script setup>
import AppsIcon from '@/components/Icons/AppsIcon.vue'
import { Popover, createResource } from 'frappe-ui'

const apps = createResource({
  url: 'frappe.apps.get_apps',
  cache: 'apps',
  auto: true,
  transform: (data) => {
    let _apps = [
      {
        name: 'frappe',
        logo: '/assets/crm/images/desk.png',
        title: __('Desk'),
        route: '/app',
      },
    ]
    data.map((app) => {
      if (app.name === 'crm') return
      _apps.push({
        name: app.name,
        logo: app.logo,
        title: __(app.title),
        route: app.route,
      })
    })

    return _apps
  },
})
</script>
