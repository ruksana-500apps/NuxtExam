<template>
  <div class="row-span-3 col-span-4 bg-white h-[calc(100vh-150px)]">
    <!-- Start of form -->
    <form @submit.prevent="save" @reset.prevent="cancel">
      <div class="mx-2 mt-3">
        <div class="text-center mb-0 rounded-0">
          <div class="flex justify-between items-center">
            <!-- Start of name field -->
            <input
              id="name"
              v-model="localTemplate.name"
              type="text"
              name="name"
              class="block rounded-md border-0 py-2 px-3 shadow-sm ring-1 ring-inset ring-gray-300 w-full placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-blue-300 sm:text-sm sm:leading-6 mt-2 "
              placeholder="Enter Template Name"
              required
            />

            <!-- End of name field -->
          </div>
        </div>
      </div>
      <div class="mx-2 mt-3">
        <div class="flex justify-between items-center">
          <!-- Start of subject field -->
          <input
            v-model="localTemplate.subject"
            type="text"
            class="block px-3 rounded-md border-0 py-2 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-blue-300 sm:text-sm sm:leading-6 w-full"
            placeholder="Enter Template Subject"
            required
          />
          <!-- End of subject field -->
        </div>
        <!-- Start of template body  -->
        <div class="mt-3">
        <textarea
          v-model="localTemplate.body"
          rows="4"
          class="p-4 h-[calc(100vh-350px)] block w-full rounded-md border-0 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-blue-300 sm:py-1.5 sm:text-sm sm:leading-6"
          placeholder="Add Template Body..."
        />
        </div>
        <!-- End of template body -->
      </div>
      <!-- Start of buttons-->
      <div class="flex justify-end mr-3 mt-4">
        <button
          type="reset"
          class="border rounded-md bg-white py-2 px-3 text-sm font-semibold text-gray-600 shadow-sm hover:bg-gray-50 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-blue-600 mr-3"
        >
          Cancel
        </button>
        <!-- shows update and save buttons -->
        <button
          type="submit"
          class="rounded-md bg-blue-600 py-2 px-3 text-sm font-semibold text-white shadow-sm hover:bg-white-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-white-600"
        >
          {{ localTemplate.uid ? "Update" : "Save" }}
        </button>
      </div>
      <!-- Ends of buttons -->
    </form>
    <!-- End of form -->
  </div>
</template>

<script setup lang="ts">
// Define props

interface Template{
    template:Object
}
const props = withDefaults(defineProps<Template>(), {
     // Template currentTemplate/selectedTemplate
    template:()=>{
        return {}
    }
});

const localTemplate: any = ref({});

// Define emits
const emit = defineEmits(["save", "edit"]);

// Save the data when click on update or save
const save = () => {
  if (
    localTemplate.value.name &&
    localTemplate.value.subject &&
    localTemplate.value.body
  ) {
    emit(localTemplate.value.uid ? "edit" : "save", localTemplate.value);
  }
};

// Show previous template when click on cancel
const cancel = () => {
  localTemplate.value = JSON.parse(JSON.stringify(props.template));
};

// Watch for localTemplate changes
watch(
  () => props.template,
  (newValue) => {
    if (newValue) {
      localTemplate.value = JSON.parse(JSON.stringify(newValue));
    }
  }
);
</script>
<style scoped></style>
