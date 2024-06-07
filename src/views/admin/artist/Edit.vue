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
              <div>
                <div class="mt-3 sm:mt-5">
                  <DialogTitle as="h3" class="text-base font-semibold leading-6 text-gray-900">
                    {{ artist.name }}
                  </DialogTitle>
                  <div class="mt-2">
                    <div class="grid max-w-2xl grid-cols-1 gap-x-6 gap-y-4 sm:grid-cols-6">
                      <div class="sm:col-span-8">
                        <label
                          for="name"
                          class="block text-sm font-medium leading-6 text-gray-900"
                        >
                          Name
                        </label>
                        <div class="mt-2">
                          <div
                            class="flex rounded-md shadow-sm ring-1 ring-inset ring-gray-300 focus-within:ring-2 focus-within:ring-inset focus-within:ring-indigo-600 sm:max-w-md"
                          >
                            <input
                              type="text"
                              name="name"
                              id="name"
                              class="block flex-1 border-0 bg-transparent py-1.5 pl-1 text-gray-900 placeholder:text-gray-400 focus:ring-0 sm:text-sm sm:leading-6"
                              v-model="editedArtist.name"
                            />
                          </div>
                        </div>
                      </div>

                      <!-- Avatar input -->
                      <div class="sm:col-span-4">
                        <label
                          for="avatar"
                          class="block text-sm font-medium leading-6 text-gray-900"
                        >
                          Avatar
                        </label>
                        <div class="mt-2">
                          <div
                            class="flex rounded-md shadow-sm ring-1 ring-inset ring-gray-300 focus-within:ring-2 focus-within:ring-inset focus-within:ring-indigo-600 sm:max-w-md"
                          >
                            <input
                              type="file"
                              name="avatar"
                              id="avatar"
                              accept="image/*"
                              @change="handleFileUpload"
                              class="block flex-1 border-0 bg-transparent py-1.5 pl-1 text-gray-900 placeholder:text-gray-400 focus:ring-0 sm:text-sm sm:leading-6"
                            />
                          </div>
                        </div>
                      </div>
                    </div>

                    <div class="text-sm text-gray-500">
                      <ul>
                        <li>name: {{ editedArtist.name }}</li>
                        <li>avatar: {{ editedArtist.avatar }}</li>
                        <!-- Add other edited fields as necessary -->
                      </ul>
                    </div>
                  </div>
                </div>
              </div>
              <div class="mt-5 sm:mt-6 flex gap-8">
                <RouterLink
                  type="button"
                  class="mt-3 inline-flex w-full justify-center rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50 sm:col-start-1 sm:mt-0"
                  to="/admin/artist"
                  ref="cancelButtonRef"
                >
                  Back
                </RouterLink>
                <button
                  type="button"
                  class="inline-flex w-full justify-center rounded-md bg-indigo-600 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600 sm:col-start-2"
                  @click="saveChanges"
                >
                  Save
                </button>
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

const artist = ref({
  id: null,
  name: '',
  number_of_tracks: '',
  year: '',
  album_art: '',
  artist: {
    id: '',
    name: ''
  },
  studio: '',
  genre: '',
  sort_order: '',
  status: true
})

const editedArtist = ref({
  name: '',
  genre: '',
  avatar: null, // Add avatar field
  // Add other fields here that you want to edit
})

// Fetch the artist data from the API
const fetchArtist = async () => {
  try {
    const response = await fetch(`https://tft9n9vry9.execute-api.ap-southeast-1.amazonaws.com/Stage1/artists/?id=${route.params.id}`)
    const data = await response.json()
    artist.value = data.body
    // Set the editedArtist values to match the fetched data
    editedArtist.value = { ...artist.value }
  } catch (error) {
    console.error('Error fetching artist:', error)
  }
}

onMounted(() => {
  fetchArtist()
})

// Handle file upload for the avatar input
const handleFileUpload = (event) => {
  const file = event.target.files[0]
  editedArtist.value.avatar = file
}

// Save the changes made by the user
const saveChanges = () => {
  artist.value = { ...editedArtist.value }
  console.log('Changes saved:', artist.value)
  router.push('/admin/artist') // Redirect back to artist list page
}
</script>
