<script setup lang="ts">
import { ref, toRefs, watch } from 'vue'
import { type Event } from '@/types'
import EventService from '@/services/EventService'

const props = defineProps<{
  event: Event
}>()
const { event } = toRefs(props)
const imageUrls = ref<string[]>([])
watch(
  event,
  (newEvent) => {
    EventService.getEventImages(newEvent.images ?? []).then((urls) => {
      imageUrls.value = urls
    })
  },
  { immediate: true },
)
</script>
<template>
  <p>{{ event.title }} @ {{ event.location }}</p>
  <p>{{ event.description }}</p>
  <div class="flex flex-row flex-wrap justify-center">
    <img v-for="image in imageUrls" :key="image" :src="image" alt="events image"
      class="border-solid border-gray-200 border-2 rounded p-1 m-1 w-40 hover:shadow-lg" />
  </div>
</template>
