<template>
    <div class="text-left px-10 pt-10 pb-10 block p-12 min-w-fit rounded-lg shadow-lg bg-white border border-gray-200 dark:border-gray-700 max-w-lg m-auto">
      <h5 class="mb-2 font-bold text-xl leading-6 dark:text-white">New Restaurant</h5>
      <h5 class="mb-2 font-medium text-sm leading-5 dark:text-white text-gray-500 pb-4">Please fill out the following fields and click post to add a new restaurant to the app</h5>
      <form @submit.prevent="emitSubmit">
        <div class="grid grid-cols-2 gap-4">
            <div class="form-group mb-6 text-left">
                <label for="name" class="form-label inline-block mb-2 text-gray-700">Restaurant Name</label>
                <input v-model="restData[0]" required type="text" class="form-control block
                    w-full
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
                    placeholder="Ruby's Cafe">
            </div>
            <div class="block form-group mb-6 text-left pb-2">
                <label for="address" class="form-label inline-block mb-2 text-gray-700">Street Address</label>
                <input v-model="restData[1]" required type="text" class="form-control
                w-full
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
                    focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none" id="address"
                    placeholder="198 E 11th St">
            </div>
        </div>
        <div class="grid grid-cols-2 gap-4">
            <div class="block form-group mb-6 text-left pb-2">
                <label for="website" class="form-label inline-block mb-2 text-gray-700">Website</label>
                <input v-model="restData[2]" required type="url" class="form-control
                w-full
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
                    focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none" id="website"
                    placeholder="https://littlerubyscafe.com/">
            </div>
            <div class="block form-group mb-6 text-left pb-2">
                <label for="phone" class="form-label inline-block mb-2 text-gray-700">Phone Number</label>
                <input v-model="restData[3]" required type="text" class="form-control
                w-full
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
                    focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none" id="phone"
                    placeholder="(646) 998-4265">
            </div>
        </div>
        <div class="block form-group mb-6 text-left pb-2">
            <label for="website" class="form-label inline-block mb-2 text-gray-700">Keywords</label>
            <h5 class="mb-2 font-medium text-sm leading-5 dark:text-white text-gray-500 pb-4">Please some key words to describe the restaurant, seperated by commas</h5>
            <input v-model="restData[4]" required type="text" class="form-control
                w-full
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
                focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none" id="keywords"
                placeholder="Australian, brunch">
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
          ease-in-out">Add Restaurant</button>
      </form>
    </div>
</template>
  
  <script>
  export default {
    name: 'RestForm',
    data() {
        return {
            restData: []
        }
    },
    methods: {
      emitSubmit() {
        fetch("http://localhost:3000/api/addRestaurant", {
          method: "post",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify({name: this.restData[0], address: this.restData[1], website: this.restData[2], phone: this.restData[3], keywords: this.restData[4]})
        })
        .then(response => response.json())
        .then(() => this.$emit("restSubmitted", this.restData))
        .catch(err => console.log("Error:", err));
      }
    }
  }
  </script>