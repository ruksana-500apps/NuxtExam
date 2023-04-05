<template>
  <!-- Start of loading -->
  <div v-if="pending" class="flex items-center justify-center">
    <div
      class="inline-block h-8 w-8 animate-spin rounded-full border-4 border-solid border-current border-r-transparent align-[-0.125em] motion-reduce:animate-[spin_1.5s_linear_infinite]"
      role="status"
    />
  </div>
  <!-- End of loading-->

  <div v-else>
      <!-- Shows list of notes -->
    <h1 class="text-b ml-3 px-3 mx-5">Notes</h1>
    <NotesList :NotesData="NotesData" @edit="edit" @delete="deleteItem" />
    <NotesAdd @save="save" :key="render"/>
    <NotesEdit v-if="editNote.uid" :editNote="editNote" />
    <div v-show="!NotesData.length">No Notes Found</div>
  </div>
</template>

<script setup lang="ts">
interface Notes {
  projectId: string;
  entityId: string;
  entity: string;
  note: string;
  url: string;
}
// Define Props
const props = withDefaults(defineProps<Notes>(), {
  projectId: "1",
  entityId: "1",
  entity: "CONTACTS",
  note: "",
  url: "https://v1-orm-lib.mars.hipso.cc/notes",
});
const editNote =ref({})
const render=ref(0)
//Authorization token
const authHeader = {
  Authorization:
    "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1IjoiNzVkODk5NzY5OTk2NDQzMWFlN2QyYTcwYTc4ODk1YmUiLCJkIjoiMTY3OTk5OCIsInIiOiJzYSIsInAiOiJmcmVlIiwiYSI6ImZpbmRlci5pbyIsImwiOiJ1czEiLCJleHAiOjE2ODMwOTUwMjN9.bBWtAovyIk5i3in0UhiH3BzsOIpQSlAWAIxqPwYzmxw",
};
const { pending, data: NotesData } = useLazyFetch(
  `${props.url}/entity/${props.entity}/${props.entityId}?project_id=${props.projectId}&offset=0&limit=100&sort_column=id&sort_direction=desc`,
  { method: "GET", headers: authHeader }
);

// Add notes
const save=(note:string)=>{
const postData=ref({entity_id:props.entityId,project_id:props.projectId,entity:props.entity,note:note})
useLazyFetch(`${props.url}/${props.entityId}/${props.projectId}`,{method:'POST',headers:authHeader,body: postData.value})
NotesData.value.unshift(postData.value)
render.value++

}

// Edit notes
const edit = (note: any) => {
   editNote.value=note
   console.log("editNote",  editNote.value)
};

// Delete the note based on uid
const deleteItem = (noteUid: string) => {
const {pending, data:response}=useLazyFetch(`${props.url}/${noteUid}`,{method:'DELETE',headers:authHeader})
 if (response) {
        const index = NotesData.value.findIndex((template: any) => template.uid === noteUid)
        if (index !== -1) {
            NotesData.value.splice(index, 1)
        }
    }
};
</script>
