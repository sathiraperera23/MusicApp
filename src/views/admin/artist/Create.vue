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
        <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"></div>
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
              <form v-on:submit.prevent="saveAlbum">
                <div class="grid max-w-2xl grid-cols-1 gap-x-6 gap-y-4 sm:grid-cols-6">
                  <div class="sm:col-span-8">
                    <label for="name" class="block text-sm font-medium leading-6 text-gray-900">Name</label>
                    <div class="mt-2">
                      <input type="text" name="name" id="name" v-model="album.name" class="block w-full border-gray-300 rounded-md shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm">
                    </div>
                  </div>

                  <div class="sm:col-span-8">
                    <label for="name" class="block text-sm font-medium leading-6 text-gray-900">Avatar</label>
                    <div class="mt-2">
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
                        </div>                    </div>
                  </div></div>

                <div class="mt-5 sm:mt-6 flex gap-8">
                  <button type="button" class="mt-3 inline-flex w-full justify-center rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50 sm:col-start-1 sm:mt-0" @click="open = false">Back</button>
                  <button type="submit" class="inline-flex w-full justify-center rounded-md bg-indigo-600 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600 sm:col-start-2">Save</button>
                </div>
              </form>
            </DialogPanel>
          </TransitionChild>
        </div>
      </div>
    </Dialog>
  </TransitionRoot>
</template>

<script setup>
import { ref } from 'vue'
import { Dialog, DialogPanel, DialogTitle, TransitionChild, TransitionRoot } from '@headlessui/vue'
import { useRouter } from 'vue-router'

const open = ref(true)
const router = useRouter()

const album = ref({
  name: '',
  number_of_tracks: 0,
  year: '',
  album_art: '',
  artist: {
    id: 0,
    name: '',
    avatar: ''
  },
  studio: '',
  genre: '',
  sort_order: 0,
  status: false
})
const handleFileUpload = (event) => {
  const file = event.target.files[0]
  album.value.avatar = file
}
const saveAlbum = async () => {
  try {
    const response = await fetch('https://your-api-endpoint.com/albums', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(album.value)
    })
    if (response.ok) {
      console.log('Album saved successfully:', album.value)
      // Optionally, you can reset the album form fields after saving
      album.value = {
        name: '',
        number_of_tracks: 0,
        year: '',
        album_art: '',
        artist: {
          id: 0,
          name: '',
          avatar: ''
        },
        studio: '',
        genre: '',
        sort_order: 0,
        status: false
      }
      // Route the user to the album list page
      router.push('/admin/album')
    } else {
      throw new Error('Failed to save album')
    }
  } catch (error) {
    console.error('Error saving album:', error)
  }
}
</script>

<style scoped>
/* Add your styles here */
</style>