<template>
  <TransitionRoot as="template" :show="open">
    <Dialog as="div" class="relative z-10" @close="open = false">
      <TransitionChild
        as="template"
        enter="ease-out duration-300"
        enter-from="opacity-0"
        enter-to="opacity-100"
        leave="ease-in duration-200"
        leave-from="opacity-100"
        leave-to="opacity-0"
      >
        <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity" />
      </TransitionChild>

      <div class="fixed inset-0 z-10 w-screen overflow-y-auto">
        <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
          <TransitionChild
            as="template"
            enter="ease-out duration-300"
            enter-from="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
            enter-to="opacity-100 translate-y-0 sm:scale-100"
            leave="ease-in duration-200"
            leave-from="opacity-100 translate-y-0 sm:scale-100"
            leave-to="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
          >
            <DialogPanel class="relative transform overflow-hidden rounded-lg bg-white px-4 pb-4 pt-5 text-left shadow-xl transition-all sm:my-8 sm:w-full sm:max-w-lg sm:p-6">
              <div v-if="track">
                <div class="mt-3 sm:mt-5">
                  <DialogTitle as="h3" class="text-base font-semibold leading-6 text-gray-900">
                    {{ track.name }}
                  </DialogTitle>
                  <div class="mt-2">
                    <div class="text-sm text-gray-500">
                      <ul>
                        <li>Name: {{ track.name }}</li>
                        <li>File Path: {{ track.file }}</li>
                        <li>Genre: {{ track.genre }}</li>
                        <li>Duration: {{ track.duration }} seconds</li>
                        <li>Artist ID: {{ track.artist }}</li>
                        <li>Album ID: {{ track.album }}</li>
                        <li>Sort Order: {{ track.sort_order }}</li>
                        <li>Status: {{ track.status ? 'Published' : 'Unpublished' }}</li>
                      </ul>
                    </div>
                  </div>
                </div>
              </div>
              <div v-else>Loading...</div>
              <div class="mt-5 sm:mt-6">
                <RouterLink
                  type="button"
                  class="mt-3 inline-flex w-full justify-center rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50 sm:col-start-1 sm:mt-0"
                  to="/admin/tracks"
                  ref="cancelButtonRef"
                >
                  Back
                </RouterLink>
              </div>
            </DialogPanel>
          </TransitionChild>
        </div>
      </div>
    </Dialog>
  </TransitionRoot>
</template>
<script setup>
import { ref, onMounted } from 'vue'
import { Dialog, DialogPanel, DialogTitle, TransitionChild, TransitionRoot } from '@headlessui/vue'
import { useRouter, useRoute } from 'vue-router'

const open = ref(true)
const router = useRouter()
const route = useRoute()

const track = ref(null)

// Fetch the track data from the API
const fetchTrack = async () => {
  try {
    const response = await fetch(`https://tft9n9vry9.execute-api.ap-southeast-1.amazonaws.com/Stage1/tracks/?id=${route.params.id}`)
    const data = await response.json()
    console.log('Fetched track data:', data.body) // Debugging line to check API response
    track.value = data.body[0] // Assuming API returns a single track object
  } catch (error) {
    console.error('Error fetching track:', error)
  }
}

onMounted(() => {
  fetchTrack()
})
</script>
