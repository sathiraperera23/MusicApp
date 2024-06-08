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
                    Sign up for an account
                  </DialogTitle>
                  <div class="mt-2">
                    <form @submit.prevent="handleSubmit" class="space-y-6">
                      <div class="rounded-md shadow-sm -space-y-px">
                        <div>
                          <label for="first_name" class="sr-only">First Name</label>
                          <input
                            id="first_name"
                            name="first_name"
                            type="text"
                            required
                            v-model="first_name"
                            class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-t-md focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
                            placeholder="First Name"
                          />
                          <p v-if="errors.first_name" class="text-red-600 text-sm">{{ errors.first_name }}</p>
                        </div>
                        <div>
                          <label for="last_name" class="sr-only">Last Name</label>
                          <input
                            id="last_name"
                            name="last_name"
                            type="text"
                            required
                            v-model="last_name"
                            class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
                            placeholder="Last Name"
                          />
                          <p v-if="errors.last_name" class="text-red-600 text-sm">{{ errors.last_name }}</p>
                        </div>
                        <div>
                          <label for="email" class="sr-only">Email address</label>
                          <input
                            id="email"
                            name="email"
                            type="email"
                            required
                            v-model="email"
                            class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
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
                            required
                            v-model="password"
                            class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
                            placeholder="Password"
                          />
                          <p v-if="errors.password" class="text-red-600 text-sm">{{ errors.password }}</p>
                        </div>
                        <div>
                          <label for="age" class="sr-only">Age</label>
                          <input
                            id="age"
                            name="age"
                            type="number"
                            required
                            v-model="age"
                            class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
                            placeholder="Age"
                          />
                          <p v-if="errors.age" class="text-red-600 text-sm">{{ errors.age }}</p>
                        </div>
                        <div>
                          <label for="country" class="sr-only">Country</label>
                          <input
                            id="country"
                            name="country"
                            type="text"
                            required
                            v-model="country"
                            class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
                            placeholder="Country"
                          />
                          <p v-if="errors.country" class="text-red-600 text-sm">{{ errors.country }}</p>
                        </div>
                        <div>
                          <label for="subscription" class="sr-only">Subscription</label>
                          <input
                            id="subscription"
                            name="subscription"
                            type="text"
                            required
                            v-model="subscription"
                            class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
                            placeholder="Subscription"
                          />
                          <p v-if="errors.subscription" class="text-red-600 text-sm">{{ errors.subscription }}</p>
                        </div>
                        <div>
                          <label for="phone" class="sr-only">Phone</label>
                          <input
                            id="phone"
                            name="phone"
                            type="text"
                            required
                            v-model="phone"
                            class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
                            placeholder="Phone"
                          />
                          <p v-if="errors.phone" class="text-red-600 text-sm">{{ errors.phone }}</p>
                        </div>
                      </div>

                      <div>
                        <button
                          type="submit"
                          class="group relative w-full flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
                        >
                          Sign Up
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
                  to="/view/admin"
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
const first_name = ref('')
const last_name = ref('')
const email = ref('')
const password = ref('')
const age = ref('')
const country = ref('')
const subscription = ref('')
const phone = ref('')
const errors = ref({})

const validate = () => {
  const newErrors = {}

  if (!first_name.value) {
    newErrors.first_name = 'First name is required.'
  }
  if (!last_name.value) {
    newErrors.last_name = 'Last name is required.'
  }
  const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  if (!email.value || !emailPattern.test(email.value)) {
    newErrors.email = 'Valid email is required.'
  }
  if (!password.value) {
    newErrors.password = 'Password is required.'
  } else if (password.value.length < 6) {
    newErrors.password = 'Password must be at least 6 characters.'
  }
  if (!age.value) {
    newErrors.age = 'Age is required.'
  }
  if (!country.value) {
    newErrors.country = 'Country is required.'
  }
  if (!subscription.value) {
    newErrors.subscription = 'Subscription is required.'
  }
  if (!phone.value) {
    newErrors.phone = 'Phone is required.'
  }

  errors.value = newErrors
  return Object.keys(newErrors).length === 0
}

const handleSubmit = async () => {
  if (validate()) {
    try {
      const response = await fetch('https://your-backend-endpoint.com/signup', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          first_name: first_name.value,
          last_name: last_name.value,
          email: email.value,
          password: password.value,
          age: age.value,
          country: country.value,
          subscription: subscription.value,
          phone: phone.value,
        }),
      })

      if (!response.ok) {
        throw new Error('Failed to sign up.')
      }

      const data = await response.json()
      console.log('Sign up successful:', data)
      // Handle successful sign-up (e.g., redirect to login page, show success message, etc.)
    } catch (error) {
      console.error('Error signing up:', error)
      // Handle error (e.g., show error message)
    }
  }
}
</script>

<style scoped>
/* Add your styles here */
</style>
