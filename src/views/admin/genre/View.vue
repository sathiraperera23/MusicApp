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
        <div
          class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0"
        >
          <TransitionChild
            as="template"
            enter="ease-out duration-300"
            enter-from="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
            enter-to="opacity-100 translate-y-0 sm:scale-100"
            leave="ease-in duration-200"
            leave-from="opacity-100 translate-y-0 sm:scale-100"
            leave-to="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
          >
            <DialogPanel
              class="relative transform overflow-hidden rounded-lg bg-white px-4 pb-4 pt-5 text-left shadow-xl transition-all sm:my-8 sm:w-full sm:max-w-lg sm:p-6"
            >
              <div v-if="fetchedGenre">
                <div class="mt-3 sm:mt-5">
                  <DialogTitle as="h3" class="text-base font-semibold leading-6 text-gray-900">
                    {{ fetchedGenre.name }}
                  </DialogTitle>
                  <div class="mt-2">
                    <div class="text-sm text-gray-500">
                      <ul>
                        <li>Description: {{ fetchedGenre.description }}</li>
                        <li>Image: <img :src="fetchedGenre.image" alt="Genre Image" class="h-24 w-24 object-cover rounded-full" /></li>
                        <li>Sort Order: {{ fetchedGenre.sort_order }}</li>
                        <li>Status: {{ fetchedGenre.status }}</li>
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
                  to="/admin/genre"
                  ref="cancelButtonRef"
                >
                  Close
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
const fetchedGenre = ref(null)

onMounted(async () => {
  try {
    const response = await fetch(`https://tft9n9vry9.execute-api.ap-southeast-1.amazonaws.com/Stage1/genres/?id=${route.params.id}`)
    const data = await response.json()
    console.log('Fetched genre:', data.body) // Add this line for debugging
    fetchedGenre.value = data.body[0]
  } catch (error) {
    console.error('Error fetching genre:', error)
  }
})
</script>

<style scoped>
/* Add your styles here */
</style>
