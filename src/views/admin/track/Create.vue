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
              <form v-on:submit.prevent="saveSong">
                <div class="grid max-w-2xl grid-cols-1 gap-x-6 gap-y-4 sm:grid-cols-6">
                  <!-- Input fields -->
                  <div class="sm:col-span-8">
                    <label for="name" class="block text-sm font-medium leading-6 text-gray-900">Name</label>
                    <div class="mt-2">
                      <input type="text" name="name" id="name" v-model="song.name" class="block w-full border-gray-300 rounded-md shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm">
                    </div>
                  </div>
                  <div class="sm:col-span-8">
                    <label for="file" class="block text-sm font-medium leading-6 text-gray-900">File Path</label>
                    <div class="mt-2">
                      <input type="text" name="file" id="file" v-model="song.file" class="block w-full border-gray-300 rounded-md shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm">
                    </div>
                  </div>
                  <div class="sm:col-span-8">
                    <label for="genre" class="block text-sm font-medium leading-6 text-gray-900">Genre</label>
                    <div class="mt-2">
                      <input type="number" name="genre" id="genre" v-model="song.genre" class="block w-full border-gray-300 rounded-md shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm">
                    </div>
                  </div>
                  <div class="sm:col-span-8">
                    <label for="duration" class="block text-sm font-medium leading-6 text-gray-900">Duration</label>
                    <div class="mt-2">
                      <input type="number" name="duration" id="duration" v-model="song.duration" class="block w-full border-gray-300 rounded-md shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm">
                    </div>
                  </div>
                  <div class="sm:col-span-8">
                    <label for="artist" class="block text-sm font-medium leading-6 text-gray-900">Artist</label>
                    <div class="mt-2">
                      <input type="number" name="artist" id="artist" v-model="song.artist" class="block w-full border-gray-300 rounded-md shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm">
                    </div>
                  </div>
                  <div class="sm:col-span-8">
                    <label for="album" class="block text-sm font-medium leading-6 text-gray-900">Album</label>
                    <div class="mt-2">
                      <input type="number" name="album" id="album" v-model="song.album" class="block w-full border-gray-300 rounded-md shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm">
                    </div>
                  </div>
                  <div class="sm:col-span-8">
                    <label for="sort_order" class="block text-sm font-medium leading-6 text-gray-900">Sort Order</label>
                    <div class="mt-2">
                      <input type="number" name="sort_order" id="sort_order" v-model="song.sort_order" class="block w-full border-gray-300 rounded-md shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm">
                    </div>
                  </div>
                  <div class="sm:col-span-8">
                    <label for="status" class="block text-sm font-medium leading-6 text-gray-900">Status</label>
                    <div class="mt-2">
                      <input type="checkbox" name="status" id="status" v-model="song.status" class="rounded text-indigo-500 border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm">
                    </div>
                  </div>
                </div>

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

const song = ref({
  name: '',
  file: '',
  genre: 1,
  duration: 0,
  artist: 1,
  album: 1,
  sort_order: 0,
  status: true
})

const saveSong = async () => {
  try {
    const response = await fetch('https://tft9n9vry9.execute-api.ap-southeast-1.amazonaws.com/Stage1/tracks', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(song.value)
    })

    if (response.ok) {
      console.log('Song saved successfully:', song.value)
      // Optionally, you can reset the form fields after saving
      song.value = {
        name: '',
        file: '',
        genre: 1,
        duration: 0,
        artist: 1,
        album: 1,
        sort_order: 0,
        status: true
      }
      // Route the user to the song list page
      router.push('/admin/songs')
    } else {
      throw new Error('Failed to save song')
    }
  } catch (error) {
    console.error('Error saving song:', error)
  }
}
</script>

<style scoped>
/* Add your styles here */
</style>
