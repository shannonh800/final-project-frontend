<template>
    <div class="text-left px-10 block pt-10 pb-10 rounded-lg shadow-lg bg-white border border-gray-200 dark:border-gray-700 max-w-lg m-auto">
      <h5 class="mb-2 font-bold text-xl leading-6 dark:text-white">Add A Review</h5>
      <h5 class="mb-2 font-medium text-sm leading-5 dark:text-white text-gray-500 pb-4">Please fill out the following fields and click publish to post your review for this restaurant</h5>
      <form @submit.prevent="emitSubmit">
        <div class="form-group mb-4">
          <h5 class="mb-2 font-bold text-lg leading-6 dark:text-white">Restaurant Name: {insert restaurant name}</h5>
        </div>
        <div class="form-group mb-4 text-left">
          <label for="rating" class="form-label inline-block mb-2 text-gray-700">Rating (out of 5.0)</label>
          <input v-model="reviewData[0]" required type="number" min="0.0" max="5.0" step="0.1" class="form-control block
            w-fit
            px-3
            py-1.5
            text-base
            font-normal
            text-gray-700
            bg-white bg-clip-padding
            border border-solid border-gray-300
            rounded
            transition
            ease-in-out
            m-0
            focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none" id="rating"
            placeholder="4.8">
        </div>
        <div class="block form-group mb-4 pb-2 text-left">
          <label for="commentary" class="form-label inline-block mb-2 text-gray-700">Commentary</label>
          <textarea v-model="reviewData[1]" required type="text" class="form-control
            block
            w-full
            h-32
            align-text-top
            px-3
            py-1.5
            text-base
            font-normal
            text-gray-700
            bg-white bg-clip-padding
            border border-solid border-gray-300
            rounded
            transition
            ease-in-out
            m-0
            focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none" id="commentary" rows="3"
            placeholder="Best shrimp pasta I've ever had. And the truffle fries are a must!">
          </textarea>
        </div>
        <button type="submit" class="
          px-6
          py-2.5
          bg-blue-600
          text-white
          font-medium
          text-xs
          leading-tight
          uppercase
          rounded
          shadow-md
          hover:bg-blue-700 hover:shadow-lg
          focus:bg-blue-700 focus:shadow-lg focus:outline-none focus:ring-0
          active:bg-blue-800 active:shadow-lg
          transition
          duration-150
          ease-in-out">Post Review</button>
      </form>
    </div>
</template>
  
  <script>
  export default {
    name: 'ReviewForm',
    data() {
      return {
        reviewData: []
      }
    },
    props: {
      restName: {
        required: true,
        type: String
      }
    },
    methods: {
      emitSubmit() {
        console.log("Submitting Review");
        fetch("http://localhost:3000/api/addReview", {
          method: "post",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify({restaurant: this.restName, rating: this.reviewData[0], commentary: this.reviewData[1]})
        })
        .then(response => response.json())
        .then(() => {this.$emit("reviewSubmitted")});
      }
    }
  }
  </script>