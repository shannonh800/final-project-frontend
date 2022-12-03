<template>
  <div class="max-w-xl max-h-fit mx-auto px-4 pt-2 pb-4">
    <a href="#" @click="emitClicked" class='m-0 h-40 max-w-5xl flex bg-white rounded-lg shadow-md hover:bg-gray-100 dark:bg-gray-800 dark:border-gray-700 dark:hover:bg-gray-700'>
      <div class='flex-1 card-block text-left rounded-lg hover:bg-gray-100'>
        <div class="pt-5 pb-0 pl-14 pr-60">
          <p class='underline font-bold text-xl mb-3 mt-2 leading-3'>{{restaurant.name}}</p>
          <div class="flex flex-row items-center">
            <p class="text-sm text-grey font-bold  mr-1 mt-0 leading-2">{{restaurant.rating}}</p>
            <svg aria-hidden="true" class="mt-0 leading-2 w-5 h-5 text-yellow-400" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><title>Rating star</title><path d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"></path></svg>
          </div>
          <p class="text-sm text-grey block mt-4 leading-3">Address: {{restaurant.address}}</p>
          <p class="text-sm text-grey block mt-4 leading-none">Type of Food: {{restaurant.keywords}}</p>
        </div>
      </div>
    </a>
  </div>
</template>

<script>
export default {
  name: 'RestaurantCard',
  props: {
      restaurant: {
        required: true,
        type: Object
      }
  },
  methods: {
    emitClicked() {
      this.$emit("restClicked");
    }
  }
}
</script>