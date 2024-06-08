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
                    Log in to your account
                  </DialogTitle>
                  <div class="mt-2">
                    <form @submit.prevent="handleLogin" class="space-y-6">
                      <div class="rounded-md shadow-sm -space-y-px">
                        <div>
                          <label for="email" class="sr-only">Email address</label>
                          <input
                            id="email"
                            name="email"
                            type="email"
                            autocomplete="email"
                            required
                            v-model="email"
                            class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-t-md focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
                            placeholder="Email address"
                          />
                          <p v-if="errors.email" class="text-red-600 text-sm">{{ errors.email }}</p>
                        </div>
                        <div>
                          <label for="password" class="sr-only">Password</label>
                          <input
                            id="password"
                            name="password"
                            type="password"
                            autocomplete="current-password"
                            required
                            v-model="password"
                            class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-b-md focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
                            placeholder="Password"
                          />
                          <p v-if="errors.password" class="text-red-600 text-sm">{{ errors.password }}</p>
                        </div>
                      </div>

                      <div>
                        <button
                          type="submit"
                          class="group relative w-full flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
                        >
                          Sign in
                        </button>
                      </div>
                    </form>
                  </div>
                </div>
              </div>
              <div class="mt-5 sm:mt-6">
                <RouterLink
                  type="button"
                  class="mt-3 inline-flex w-full justify-center rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50 sm:col-start-1 sm:mt-0"
                  to="/"
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
import { ref } from 'vue'
import { Dialog, DialogPanel, DialogTitle, TransitionChild, TransitionRoot } from '@headlessui/vue'
import { useRouter } from 'vue-router'

const open = ref(true)
const email = ref('')
const password = ref('')
const errors = ref({})

const router = useRouter()

const validate = () => {
  const newErrors = {}

  const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  if (!email.value || !emailPattern.test(email.value)) {
    newErrors.email = 'Valid email is required.'
  }
  if (!password.value) {
    newErrors.password = 'Password is required.'
  } else if (password.value.length < 6) {
    newErrors.password = 'Password must be at least 6 characters.'
  }

  errors.value = newErrors
  return Object.keys(newErrors).length === 0
}

const handleLogin = async () => {
  if (validate()) {
    try {
      const response = await fetch('https://your-api-endpoint.com/login', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          email: email.value,
          password: password.value,
        }),
      })

      const data = await response.json()

      if (!response.ok) {
        throw new Error(data.message || 'Failed to log in.')
      }

      console.log('Login successful:', data)
      router.push('/dashboard') // Redirect to the dashboard after login
    } catch (error) {
      console.error('Error logging in:', error)
      errors.value.general = error.message || 'An error occurred while trying to log in.'
    }
  }
}
</script>

<style scoped>
/* Add your styles here */
</style>
