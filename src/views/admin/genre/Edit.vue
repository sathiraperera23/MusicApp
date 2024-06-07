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
                    {{ artist.name }}
                  </DialogTitle>
                  <div class="mt-2">
                    <div class="grid max-w-2xl grid-cols-1 gap-x-6 gap-y-4 sm:grid-cols-6">
                      <div class="sm:col-span-8">
                        <label for="name" class="block text-sm font-medium leading-6 text-gray-900">Name</label>
                        <div class="mt-2">
                          <div class="flex rounded-md shadow-sm ring-1 ring-inset ring-gray-300 focus-within:ring-2 focus-within:ring-inset focus-within:ring-indigo-600 sm:max-w-md">
                            <input type="text" name="name" id="name" class="block flex-1 border-0 bg-transparent py-1.5 pl-1 text-gray-900 placeholder:text-gray-400 focus:ring-0 sm:text-sm sm:leading-6" v-model="editedArtist.name" />
                          </div>
                        </div>
                      </div>

                      <div class="sm:col-span-8">
                        <label for="description" class="block text-sm font-medium leading-6 text-gray-900">Description</label>
                        <div class="mt-2">
                          <div class="flex rounded-md shadow-sm ring-1 ring-inset ring-gray-300 focus-within:ring-2 focus-within:ring-inset focus-within:ring-indigo-600 sm:max-w-md">
                            <textarea name="description" id="description" class="block flex-1 border-0 bg-transparent py-1.5 pl-1 text-gray-900 placeholder:text-gray-400 focus:ring-0 sm:text-sm sm:leading-6" v-model="editedArtist.description"></textarea>
                          </div>
                        </div>
                      </div>

                      <div class="sm:col-span-4">
                        <label for="image" class="block text-sm font-medium leading-6 text-gray-900">Image</label>
                        <div class="mt-2">
                          <div class="flex rounded-md shadow-sm ring-1 ring-inset ring-gray-300 focus-within:ring-2 focus-within:ring-inset focus-within:ring-indigo-600 sm:max-w-md">
                            <input type="file" name="image" id="image" accept="image/*" @change="handleFileUpload" class="block flex-1 border-0 bg-transparent py-1.5 pl-1 text-gray-900 placeholder:text-gray-400 focus:ring-0 sm:text-sm sm:leading-6" />
                          </div>
                        </div>
                      </div>

                      <div class="sm:col-span-8">
                        <label for="sort_order" class="block text-sm font-medium leading-6 text-gray-900">Sort Order</label>
                        <div class="mt-2">
                          <input type="number" name="sort_order" id="sort_order" v-model="editedArtist.sort_order" class="block w-full border-gray-300 rounded-md shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm" />
                        </div>
                      </div>

                      <div class="sm:col-span-8">
                        <label for="status" class="block text-sm font-medium leading-6 text-gray-900">Status</label>
                        <div class="mt-2">
                          <input type="checkbox" name="status" id="status" v-model="editedArtist.status" class="rounded text-indigo-500 border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm" />
                        </div>
                      </div>
                    </div>

                    <div class="text-sm text-gray-500">
                      <ul>
                        <li>Name: {{ editedArtist.name }}</li>
                        <li>Description: {{ editedArtist.description }}</li>
                        <li>Image: {{ editedArtist.image }}</li>
                        <li>Sort Order: {{ editedArtist.sort_order }}</li>
                        <li>Status: {{ editedArtist.status }}</li>
                      </ul>
                    </div>
                  </div>
                </div>
              </div>
              <div class="mt-5 sm:mt-6 flex gap-8">
                <RouterLink type="button" class="mt-3 inline-flex w-full justify-center rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50 sm:col-start-1 sm:mt-0" to="/admin/artist" ref="cancelButtonRef">Back</RouterLink>
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

const artist = ref({
  id: null,
  name: '',
  description: '',
  image: '',
  sort_order: 0,
  status: true
})

const editedArtist = ref({
  name: '',
  description: '',
  image: null, // Add image field
  sort_order: 0,
  status: true
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

// Handle file upload for the image input
const handleFileUpload = (event) => {
  const file = event.target.files[0]
  editedArtist.value.image = file
}

// Save the changes made by the user
const saveChanges = async () => {
  // Update the artist data with the edited values
  const formData = new FormData()
  formData.append('name', editedArtist.value.name)
  formData.append('description', editedArtist.value.description)
  formData.append('image', editedArtist.value.image)
  formData.append('sort_order', editedArtist.value.sort_order)
  formData.append('status', editedArtist.value.status)

  try {
    const response = await fetch(`https://tft9n9vry9.execute-api.ap-southeast-1.amazonaws.com/Stage1/artists/${route.params.id}`, {
      method: 'PUT',
      body: formData
    })

    if (response.ok) {
      console.log('Artist updated successfully:', artist.value)
      router.push('/admin/artist') // Redirect back to artist list page
    } else {
      throw new Error('Failed to update artist')
    }
  } catch (error) {
    console.error('Error updating artist:', error)
  }
}
</script>

<style scoped>
/* Add your styles here */
</style>
