<template>
  <div class="max-w-fit max-h-fit mx-auto px-4 pt-2 pb-4">
    <div @click="emitClicked" class='m-0 flex bg-white rounded-lg shadow-md hover:bg-gray-100 dark:bg-gray-800 dark:border-gray-700 dark:hover:bg-gray-700'>
      <div class='flex-no-shrink'>
        <img alt='' class='w-40 h-40 block mx-auto rounded-lg' src='https://i.pinimg.com/originals/83/a8/7f/83a87fad679449c6d463982ee446ef85.jpg'>
      </div>
      <div class='flex-1 card-block text-left rounded-lg hover:bg-gray-100'>
        <div class="pt-5 pb-0 pl-14 pr-60">
          <p class='font-bold text-lg mb-3 leading-3'>{{restaurant.name}}</p>
          <p class="text-sm text-grey font-bold block mt-6 leading-3">Restaurant Rating</p>
          <p class="text-sm text-grey block mt-6 leading-3">Address: {{restaurant.address}}</p>
          <p class="text-sm text-grey block mt-6 leading-3">Type of food: {{restaurant.keywords}}</p>
        </div>
      </div>
    </div>
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