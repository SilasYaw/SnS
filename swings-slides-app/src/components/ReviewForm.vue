<template>
  <div class="w-full p-4 bg-gray-50 dark:bg-gray-900 rounded-b-xl shadow flex flex-col gap-4">
    <h3 class="text-lg font-semibold text-gray-800 dark:text-gray-100 mb-2">Add a Review</h3>
    <form @submit.prevent="handleSubmit" class="flex flex-col gap-3">
      <div class="flex flex-col gap-1">
        <label for="author" class="text-sm text-gray-700 dark:text-gray-200">Name</label>
        <InputText v-model="form.author" id="author" type="text" required maxlength="32"
          class="rounded border border-gray-300 dark:border-gray-700 bg-white dark:bg-gray-900 text-gray-900 dark:text-gray-100 px-3 py-2 focus:outline-none focus:ring-2 focus:ring-blue-400" />
      </div>
      <div class="flex flex-col gap-1">
        <label class="text-sm text-gray-700 dark:text-gray-200">Rating</label>
        <Rating v-model="form.rating" :cancel="false" :stars="5" class="yellow-stars" />
      </div>
      <div class="flex flex-col gap-1">
        <label for="comment" class="text-sm text-gray-700 dark:text-gray-200">Comment</label>
        <InputTextarea v-model="form.comment" id="comment" required maxlength="300"
          class="rounded border border-gray-300 dark:border-gray-700 bg-white dark:bg-gray-900 text-gray-900 dark:text-gray-100 px-3 py-2 focus:outline-none focus:ring-2 focus:ring-blue-400" />
      </div>
      <Button type="submit"
        label="Submit Review"
        icon="pi pi-check"
        class="self-start bg-blue-600 hover:bg-blue-700 text-white font-semibold px-5 py-2 rounded shadow disabled:opacity-50 disabled:cursor-not-allowed"
        :disabled="!form.author || !form.rating || !form.comment"
      />
    </form>
  </div>
</template>

<script setup lang="ts">
const emit = defineEmits<{
  (e: 'submit', value: { author: string; rating: number; comment: string; date: string }): void;
}>();

interface ReviewFormState {
  author: string;
  rating: number;
  comment: string;
}

const form = ref<ReviewFormState>({ author: '', rating: 0, comment: '' });

function handleSubmit() {
  if (!form.value.author || !form.value.rating || !form.value.comment) return;
  emit('submit', {
    author: form.value.author,
    rating: form.value.rating,
    comment: form.value.comment,
    date: new Date().toISOString().slice(0, 10)
  });
  form.value = { author: '', rating: 0, comment: '' };
}
</script>

<style scoped>
.yellow-stars .p-rating-icon.pi-star-fill {
  color: #facc15 !important;
}
</style>
