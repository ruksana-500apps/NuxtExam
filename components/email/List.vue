<template>
    <div v-for="(template, index) in templates" :key="index" >
        <!-- Start of templates list-->

        <!-- Emit selected template when click on template -->
        <div @click="()=>emit('select', template)">
            <!-- Start of icon -->
           <span>
              <DocumentIcon v-if="template.type"  class="h-5 w-5 font-[500] text-gray-600 mr-2 sm:mt-[20px]"/>
           </span>
            <!-- End of icon -->
            <div class="ml-[50px] mt-[-20px]">
                <p class="text-b">
                    {{ template.name }}
                </p>
                <p class="text-gray-600">{{ template.subject }}</p>
                <p class="text-sm text-gray-600 mb-[90px]">{{ preview(template.body) }}</p>
            </div>
        </div>
        <!-- End of templates list -->

        <!-- Starts of delete icon -->
        <span>
            <TrashIcon class="h-5 w-5 text-gray-600 ml-[15rem] mt-[-120px]" @click="() => emit('delete', template.uid)" />
        </span>
        <!-- End of delete icon -->
    </div>
</template>

<script setup lang="ts">
import { EnvelopeIcon, CodeBracketIcon, DocumentIcon,TrashIcon } from '@heroicons/vue/24/outline'
interface Templates{
    templates:Array<[{}]>
}
// Define props
const props = withDefaults(defineProps<Templates>(), {
    // Template currentTemplate/selectedTemplate
    templates:()=>{
        return []
    }
})

// Define emits
const emit = defineEmits(['select', 'delete'])

// Show preview of template body
const preview = (body: string) => {
    if (body) {
        const templateBody = body.replace(/<\/?[^>]+(>|$)/g, '').trim()
        return templateBody.slice(0, 50)
    }
}

</script>
<style scoped></style>
