<template>
  <div class="flex h-screen bg-zinc-900">
    <!-- sidebar -->
    <div class="w-[338px] bg-black p-8">
      <Logo />
      <!-- today main container -->
      <p class="mb-4 mt-12 text-xs font-bold text-[#C2C2C5]">Today</p>
      <div class="ml-2 space-y-1">
        <div
          v-for="note in todaysNotes"
          class="cursor-pointer rounded-lg p-2"
          :class="{
            'bg-[#A1842C]': note.id === selectedNote.id,
            'hover:bg-[#A1842C]/50': note.id !== selectedNote.id,
          }"
          @click="selectedNote = note"
        >
          <h3 class="truncate text-sm font-bold text-[#F4F4F5]">
            {{ note.text.substring(0, 50) }}
          </h3>
          <div class="truncate leading-none text-[#D6D6D6]">
            <span class="mr-4 text-xs text-[#F4F4F5]">
              {{
                new Date(note.updatedAt).toDateString() ===
                new Date().toDateString()
                  ? "Today"
                  : new Date(note.updatedAt).toLocaleDateString()
              }}
            </span>
            <span class="truncate text-xs text-[#D6D6D6]">{{
              note.text.substring(0, 50)
            }}</span>
          </div>
        </div>
      </div>
      <!-- /today main container -->

      <!-- yesterday main container -->
      <p class="mb-4 mt-12 text-xs font-bold text-[#C2C2C5]">Yesterday</p>
      <div class="ml-2 space-y-1">
        <div
          v-for="note in yesterdaysNotes"
          class="cursor-pointer rounded-lg p-2"
          :class="{
            'bg-[#A1842C]': note.id === selectedNote.id,
            'hover:bg-[#A1842C]/50': note.id !== selectedNote.id,
          }"
          @click="selectedNote = note"
        >
          <h3 class="truncate text-sm font-bold text-[#F4F4F5]">
            {{ note.text.substring(0, 50) }}
          </h3>
          <div class="truncate leading-none text-[#D6D6D6]">
            <span class="mr-4 text-xs text-[#F4F4F5]">
              {{
                new Date(note.updatedAt).toDateString() ===
                new Date().toDateString()
                  ? "Today"
                  : new Date(note.updatedAt).toLocaleDateString()
              }}
            </span>
            <span class="truncate text-xs text-[#D6D6D6]">{{
              note.text.substring(0, 50)
            }}</span>
          </div>
        </div>
      </div>
      <!-- /yesterday main container -->

      <!-- earlier notes main container -->
      <p class="mb-4 mt-12 text-xs font-bold text-[#C2C2C5]">Earlier</p>
      <div class="ml-2 space-y-1">
        <div
          v-for="note in earlierNotes"
          class="cursor-pointer rounded-lg p-2"
          :class="{
            'bg-[#A1842C]': note.id === selectedNote.id,
            'hover:bg-[#A1842C]/50': note.id !== selectedNote.id,
          }"
          @click="selectedNote = note"
        >
          <h3 class="truncate text-sm font-bold text-[#F4F4F5]">
            {{ note.text.substring(0, 50) }}
          </h3>
          <div class="truncate leading-none text-[#D6D6D6]">
            <span class="mr-4 text-xs text-[#F4F4F5]">
              {{
                new Date(note.updatedAt).toDateString() ===
                new Date().toDateString()
                  ? "Today"
                  : new Date(note.updatedAt).toLocaleDateString()
              }}
            </span>
            <span class="truncate text-xs text-[#D6D6D6]">{{
              note.text.substring(0, 50)
            }}</span>
          </div>
        </div>
      </div>
      <!-- /earlier notes main container -->
    </div>
    <!-- /sidebar -->

    <!-- note container  -->
    <div class="w-full">
      <div class="flex w-full items-start justify-between p-8">
        <button
          class="inline-flex items-center space-x-2 text-xs font-bold text-[#C2C2C5] hover:text-white"
        >
          <PencilIcon class="#6D6D73" />
          <span>Create Note</span>
        </button>
        <button><TrashIcon class="text-[#6D6D73] hover:text-white" /></button>
      </div>
      <div class="mx-auto max-w-[437px]">
        <p class="font-playfair text-[#929292]">
          {{ new Date(selectedNote.updatedAt).toLocaleDateString() }}
        </p>
        <p class="font-playfair my-4 text-[#D4D4D4]">
          {{ selectedNote.text }}
        </p>
      </div>
    </div>
    <!-- /note container  -->
  </div>
</template>

<script setup>
const notes = ref([]);
const selectedNote = ref({});
definePageMeta({
  middleware: ["auth"],
});

const todaysNotes = computed(() => {
  return notes.value.filter((note) => {
    return (
      new Date(note.updatedAt).toDateString() === new Date().toDateString()
    );
  });
});

const yesterdaysNotes = computed(() => {
  const yesterday = new Date();
  yesterday.setDate(yesterday.getDate() - 1);
  return notes.value.filter((note) => {
    const noteDate = new Date(note.updatedAt);
    return noteDate.toDateString() === yesterday.toDateString();
  });
});

const earlierNotes = computed(() => {
  const yesterday = new Date();
  yesterday.setDate(yesterday.getDate() - 1);
  return notes.value.filter((note) => {
    const noteDate = new Date(note.updatedAt);
    return (
      noteDate < yesterday &&
      noteDate.toDateString() !== yesterday.toDateString()
    );
  });
});

onMounted(async () => {
  notes.value = await $fetch("/api/notes");
  if (notes.value.length > 0) selectedNote.value = notes.value[0];
});
</script>
