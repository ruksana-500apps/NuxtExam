<template>
  <div>
    <form @submit.prevent="()=>emit('edit', note)">
      <div class="mx-2 mt-3">
        <div class="text-center mb-0 rounded-0">
          <div class="flex justify-between items-center">
            <!-- Start of name field -->
            <input
              id="name"
              v-model="editNote"
              type="text"
              name="name"
              class="block rounded-md border-0 py-2 px-3 shadow-sm ring-1 ring-inset ring-gray-300 w-[50%] placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-blue-300 sm:text-sm sm:leading-6 mt-2"
              placeholder="Enter Template Name"
              required
            />
            <button
              type="submit"
              class="rounded-md bg-blue-600 py-2 px-3 text-sm font-semibold text-white shadow-sm hover:bg-white-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-white-600"
            >
              Update
            </button>
            <!-- End of name field -->
          </div>
        </div>
      </div>
    </form>
  </div>
</template>

<script setup lnag="ts">

// Define props
const props=defineProps({
    editNote:Object
})
const editNote=ref(props.editNote)
</script>
