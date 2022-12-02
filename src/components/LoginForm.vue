<template>
  <div class="text-left block p-6 rounded-lg shadow-lg bg-white border border-gray-200 dark:border-gray-700 max-w-sm m-auto">
    <h5 class="mb-2 font-bold text-lg leading-6 dark:text-white">Log In</h5>
    <h5 class="mb-2 font-medium text-sm leading-5 dark:text-white text-gray-500 pb-4">Please sign in with your existing credentials</h5>
    <form @submit.prevent="emitSubmit">
      <div class="form-group mb-6">
        <label for="username" class="form-label inline-block mb-2 text-gray-700">Username</label>
        <input v-model="loginData[0]" type="text" class="form-control
          block
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
          focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none" id="email"
          placeholder="shannon800">
      </div>
      <div class="form-group mb-6 text-left pb-2">
        <label for="exampleInputPassword1" class="form-label inline-block mb-2 text-gray-700">Password</label>
        <input v-model="loginData[1]" type="password" class="form-control block
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
          focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none" id="password"
          placeholder="**********">
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
        ease-in-out">Log In</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'LoginForm',
  data() {
        return {
            loginData: []
        }
    },
  methods: {
    emitSubmit() {
      this.$emit("loginClicked");
      fetch("http://localhost:3000/api/login", {
        method: "post",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({username: this.loginData[0], password: this.loginData[1]})
      })
      .catch(err => console.log("Error:", err));
    }
  }
}
</script>