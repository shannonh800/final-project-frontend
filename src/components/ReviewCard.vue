<template>
  <a class="text-left mr-44 px-4 ml-14 block min-w-full mb-3 p-6 bg-white border rounded-lg shadow-md dark:bg-gray-800 dark:border-gray-700">
      <h5 class="mb-2 font-bold text-base leading-6 dark:text-white">Rating: {{review.rating}}/5.0</h5>
      <p class="text-xs text-gray-700 dark:text-gray-400">Commentary: {{review.commentary}}</p>
  </a>
</template>

<script>
export default {
  name: 'ReviewCard',
  props: {
    review: {
      required: true,
      type: Object
    }
  }
}
</script>