<template>
    <!-- Start of a notes list -->
    <div v-for="notes in NotesData" :key="notes.uid" class="mx-5 mt-3 relative flex items-center space-x-3 rounded-lg border border-gray-300 bg-white px-6 py-3 shadow-sm focus-within:ring-2 focus-within:ring-indigo-500 focus-within:ring-offset-2 hover:border-gray-400 w-[15%]">
      <div class="min-w-0 flex-1">
          <p class="text-sm font-medium text-gray-900 ">{{ notes.note }}</p>
      </div>
      <!-- Start of an action items -->
      <div>
      <PencilSquareIcon class="h-5 w-5 font-[500] text-gray-600 mr-2  mb-[-18px] sm:ml-[70px]" @click="()=>emit('edit', notes)"/>
      <TrashIcon class="h-5 w-5 font-[500] text-gray-600 mr-2 mb-[10px] sm:ml-[100px]" @click="()=>emit('delete', notes.uid)"/>
     </div>
        <!-- End of an action items -->
    </div>
    <!-- End of a notes list -->
</template>

<script setup lang="ts">
import {TrashIcon, PencilSquareIcon} from '@heroicons/vue/24/outline'
interface NotesList{
    NotesData:Array<[{}]>
}
// Define Props
const props = withDefaults(defineProps<NotesList>(), {
NotesData:()=>{
    return []
}
})

// Define Emits
const emit =defineEmits(['edit','delete'])
</script>