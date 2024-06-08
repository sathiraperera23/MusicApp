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
              <div>
                <div class="mt-3 sm:mt-5">
                  <DialogTitle as="h3" class="text-base font-semibold leading-6 text-gray-900">
                    {{ song.name }}
                  </DialogTitle>
                  <div class="mt-2">
                    <div class="grid max-w-2xl grid-cols-1 gap-x-6 gap-y-4 sm:grid-cols-6">
                      <div class="sm:col-span-8">
                        <label for="name" class="block text-sm font-medium leading-6 text-gray-900">Name</label>
                        <div class="mt-2">
                          <div class="flex rounded-md shadow-sm ring-1 ring-inset ring-gray-300 focus-within:ring-2 focus-within:ring-inset focus-within:ring-indigo-600 sm:max-w-md">
                            <input type="text" name="name" id="name" class="block flex-1 border-0 bg-transparent py-1.5 pl-1 text-gray-900 placeholder:text-gray-400 focus:ring-0 sm:text-sm sm:leading-6" v-model="editedSong.name" />
                          </div>
                        </div>
                      </div>

                      <div class="sm:col-span-8">
                        <label for="file" class="block text-sm font-medium leading-6 text-gray-900">File</label>
                        <div class="mt-2">
                          <div class="flex rounded-md shadow-sm ring-1 ring-inset ring-gray-300 focus-within:ring-2 focus-within:ring-inset focus-within:ring-indigo-600 sm:max-w-md">
                            <input type="file" name="file" id="file" accept="audio/*" @change="handleFileUpload" class="block flex-1 border-0 bg-transparent py-1.5 pl-1 text-gray-900 placeholder:text-gray-400 focus:ring-0 sm:text-sm sm:leading-6" />
                          </div>
                        </div>
                      </div>

                      <div class="sm:col-span-8">
                        <label for="genre" class="block text-sm font-medium leading-6 text-gray-900">Genre</label>
                        <div class="mt-2">
                          <input type="number" name="genre" id="genre" v-model="editedSong.genre" class="block w-full border-gray-300 rounded-md shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm" />
                        </div>
                      </div>

                      <div class="sm:col-span-8">
                        <label for="duration" class="block text-sm font-medium leading-6 text-gray-900">Duration</label>
                        <div class="mt-2">
                          <input type="number" name="duration" id="duration" v-model="editedSong.duration" class="block w-full border-gray-300 rounded-md shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm" />
                        </div>
                      </div>

                      <div class="sm:col-span-8">
                        <label for="artist" class="block text-sm font-medium leading-6 text-gray-900">Artist</label>
                        <div class="mt-2">
                          <input type="number" name="artist" id="artist" v-model="editedSong.artist" class="block w-full border-gray-300 rounded-md shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm" />
                        </div>
                      </div>

                      <div class="sm:col-span-8">
                        <label for="album" class="block text-sm font-medium leading-6 text-gray-900">Album</label>
                        <div class="mt-2">
                          <input type="number" name="album" id="album" v-model="editedSong.album" class="block w-full border-gray-300 rounded-md shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm" />
                        </div>
                      </div>

                      <div class="sm:col-span-8">
                        <label for="sort_order" class="block text-sm font-medium leading-6 text-gray-900">Sort Order</label>
                        <div class="mt-2">
                          <input type="number" name="sort_order" id="sort_order" v-model="editedSong.sort_order" class="block w-full border-gray-300 rounded-md shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm" />
                        </div>
                      </div>

                      <div class="sm:col-span-8">
                        <label for="status" class="block text-sm font-medium leading-6 text-gray-900">Status</label>
                        <div class="mt-2">
                          <input type="checkbox" name="status" id="status" v-model="editedSong.status" class="rounded text-indigo-500 border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm" />
                        </div>
                      </div>
                    </div>

                    <div class="text-sm text-gray-500">
                      <ul>
                        <li>Name: {{ editedSong.name }}</li>
                        <li>File: {{ editedSong.file }}</li>
                        <li>Genre: {{ editedSong.genre }}</li>
                        <li>Duration: {{ editedSong.duration }}</li>
                        <li>Artist: {{ editedSong.artist }}</li>
                        <li>Album: {{ editedSong.album }}</li>
                        <li>Sort Order: {{ editedSong.sort_order }}</li>
                        <li>Status: {{ editedSong.status }}</li>
                      </ul>
                    </div>
                  </div>
                </div>
              </div>
              <div class="mt-5 sm:mt-6 flex gap-8">
                <RouterLink type="button" class="mt-3 inline-flex w-full justify-center rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50 sm:col-start-1 sm:mt-0" to="/admin/songs" ref="cancelButtonRef">Back</RouterLink>
                <button type="button" class="inline-flex w-full justify-center rounded-md bg-indigo-600 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600 sm:col-start-2" @click="saveChanges">Save</button>
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

const song = ref({
  id: null,
  name: '',
  file: '',
  genre: 0,
  duration: 0,
  artist: 0,
  album: 0,
  sort_order: 0,
  status: true
})

const editedSong = ref({
  name: '',
  file: null, // Add file field
  genre: 0,
  duration: 0,
  artist: 0,
  album: 0,
  sort_order: 0,
  status: true
})

// Fetch the song data from the API
const fetchSong = async () => {
  try {
    const response = await fetch(`https://tft9n9vry9.execute-api.ap-southeast-1.amazonaws.com/Stage1/songs/?id=${route.params.id}`)
    const data = await response.json()
    song.value = data.body
    // Set the editedSong values to match the fetched data
    editedSong.value = { ...song.value }
  } catch (error) {
    console.error('Error fetching song:', error)
  }
}

onMounted(() => {
  fetchSong()
})

// Handle file upload for the file input
const handleFileUpload = (event) => {
  const file = event.target.files[0]
  editedSong.value.file = file
}

// Save the changes made by the user
const saveChanges = async () => {
  // Update the song data with the edited values
  const formData = new FormData()
  formData.append('name', editedSong.value.name)
  formData.append('file', editedSong.value.file)
  formData.append('genre', editedSong.value.genre)
  formData.append('duration', editedSong.value.duration)
  formData.append('artist', editedSong.value.artist)
  formData.append('album', editedSong.value.album)
  formData.append('sort_order', editedSong.value.sort_order)
  formData.append('status', editedSong.value.status)

  try {
    const response = await fetch(`https://tft9n9vry9.execute-api.ap-southeast-1.amazonaws.com/Stage1/tracks/${route.params.id}`, {
      method: 'PUT',
      body: formData
    })

    if (response.ok) {
      console.log('Song updated successfully:', song.value)
      router.push('/admin/songs') // Redirect back to songs list page
    } else {
      throw new Error('Failed to update song')
    }
  } catch (error) {
    console.error('Error updating song:', error)
  }
}
</script>

<style scoped>
/* Add your styles here */
</style>
