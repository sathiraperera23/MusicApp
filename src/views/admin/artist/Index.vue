<template>
  <div class="px-4 sm:px-6 lg:px-8">
    <div class="sm:flex sm:items-center">
      <div class="sm:flex-auto">
        <h1 class="text-base font-semibold leading-6 text-gray-900">Artists</h1>
        <p class="mt-2 text-sm text-gray-700">All Artists</p>
      </div>
      <div class="mt-4 sm:ml-16 sm:mt-0 sm:flex-none">
        <RouterLink
    to="/admin/artist/create"
    class="block rounded-md bg-indigo-600 px-3 py-2 text-center text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
  >
    Add Artist
  </RouterLink>
      </div>
    </div>
    <div class="mt-8 flow-root">
      <div class="-mx-4 -my-2 overflow-x-auto sm:-mx-6 lg:-mx-8">
        <div class="inline-block min-w-full py-2 align-middle sm:px-6 lg:px-8">
          <table class="min-w-full divide-y divide-gray-300">
            <thead>
              <tr>
                <th
                  scope="col"
                  class="py-3.5 pl-4 pr-3 text-left text-sm font-semibold text-gray-900 sm:pl-0"
                >
                  ID
                </th>
                <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">
                  Name
                </th>
                <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">
                  Avatar
                </th>
                <th scope="col" class="relative py-3.5 pl-3 pr-4 sm:pr-0">
                  <span class="sr-only">Actions</span>
                </th>
              </tr>
            </thead>
            <tbody class="divide-y divide-gray-200">
              <tr v-for="artist in artists" :key="artist.id">
                <td
                  class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-0"
                >
                  {{ artist.id }}
                </td>
                <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                  {{ artist.name }}
                </td>
                <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                  {{ artist.avatar }}
                </td>
                <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                  <span v-if="artist.status">Active</span>
                  <span v-else>Inactive</span>
                </td>
                <td
                  class="relative whitespace-nowrap py-4 pl-3 pr-4 text-right text-sm font-medium sm:pr-0"
                >
                  <RouterLink
                    :to="`/admin/artist/${artist.id}`"
                    class="text-indigo-600 hover:text-indigo-900"
                  >
                    View
                    <span class="sr-only">, {{ artist.name }}</span>
                  </RouterLink>
                  |
                  <RouterLink
                    :to="`/admin/artist/${artist.id}/edit`"
                    class="text-indigo-600 hover:text-indigo-900"
                  >
                    Edit
                    <span class="sr-only">, {{ artist.name }}</span>
                  </RouterLink>
                  |
                  <button
                    type="button"
                    @click="deleteArtist(artist.id)"
                    class="text-red-600 hover:text-red-900"
                  >
                    Delete
                    <span class="sr-only">, {{ artist.name }}</span>
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
  <router-view />
</template>
<script setup>
import { ref } from 'vue'

const artists = ref([])

fetch('https://tft9n9vry9.execute-api.ap-southeast-1.amazonaws.com/Stage1/artists')
  .then((response) => response.json())
  .then((response) => {
    console.log(response)
    artists.value = response.body
  })

const deleteArtist = async (id) => {
  try {
    const response = await fetch(`https://tft9n9vry9.execute-api.ap-southeast-1.amazonaws.com/Stage1/artists?id=${id}`, {
      method: 'DELETE',
    })
    if (!response.ok) {
      throw new Error('Failed to delete artist')
    }
    // Remove the artist from the artists array
    artists.value = artists.value.filter(artist => artist.id !== id)
  } catch (error) {
    console.error('Error deleting artist:', error)
  }
}
</script>