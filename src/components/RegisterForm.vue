<template>
    <div class="text-left w-full max-w-lg p-6 rounded-lg shadow-lg bg-white border border-gray-200 dark:border-gray-700 m-auto">
        <h5 class="mb-2 font-bold text-lg leading-6 dark:text-white">Sign Up</h5>
        <h5 class="mb-2 font-medium text-sm leading-5 dark:text-white text-gray-500 pb-4">Please create a new account below</h5>
        <form @submit.prevent="emitSubmit">
            <div class="grid grid-cols-2 gap-4">
                <div class="md:col-span-1 -mx-3 mb-4">
                    <div class="w-full px-3 mb-6 md:mb-0">
                    <label class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2" for="grid-first-name">
                        Full Name
                    </label>
                    <input v-model="userData[0]" required class="appearance-none block w-full bg-gray-100 text-gray-700 border border-gray-200 rounded py-2 px-4 mb-3 leading-tight focus:outline-none focus:bg-white" id="fullName" type="text" placeholder="First Last">
                    </div>
                </div>
                <div class="flex flex-wrap -mx-3 mb-4">
                    <div class="w-full px-3 mb-6 md:mb-0">
                    <label class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2" for="grid-first-name">
                        Username
                    </label>
                    <input v-model="userData[1]" required class="appearance-none block w-full bg-gray-100 text-gray-700 border border-gray-200 rounded py-2 px-4 mb-3 leading-tight focus:outline-none focus:bg-white" id="username" type="text" placeholder="someusername123">
                    </div>
                </div>
            </div>
            <div class="flex flex-wrap -mx-3 mb-4">
                <div class="w-full px-3 mb-6 md:mb-0">
                <label class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2" for="grid-first-name">
                    Email Address
                </label>
                <input v-model="userData[2]" required class="appearance-none block w-full bg-gray-100 text-gray-700 border border-gray-200 rounded py-2 px-4 mb-3 leading-tight focus:outline-none focus:bg-white" id="email" type="email" placeholder="myname@gmail.com">
                </div>
            </div>
            <div class="flex flex-wrap -mx-3 mb-4">
                <div class="w-full px-3">
                <label class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2" for="grid-password">
                    Password
                </label>
                <input v-model="userData[3]" required class="appearance-none block w-full bg-gray-100 text-gray-700 border border-gray-200 rounded py-2 px-4 mb-3 leading-tight focus:outline-none focus:bg-white focus:border-gray-500" id="grid-password" type="password" placeholder="******************">
                <p class="text-gray-600 text-xs italic">Make it as long and as crazy as you'd like</p>
                </div>
            </div>
            <div class="flex flex-wrap -mx-3 mb-4">
                <div class="w-full md:w-1/2 px-3 mb-6 md:mb-0">
                <label class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2" for="grid-state">
                    Role
                </label>
                <div class="relative">
                    <select v-model="userData[4]" required class="block appearance-none w-full bg-gray-100 border border-gray-200 text-gray-700 py-2 px-4 pr-8 rounded leading-tight focus:outline-none focus:bg-white focus:border-gray-500" id="grid-state">
                    <option value="user">User</option>
                    <option value="admin">Admin</option>
                    </select>
                    <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700">
                    <svg class="fill-current h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"/></svg>
                    </div>
                </div>
                </div>
            </div>
            <div>
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
                ease-in-out">
                Create Account
                </button>
            </div>
        </form>
    </div>
</template>

<script>
export default {
  name: 'RegisterForm',
  data() {
        return {
            userData: []
        }
    },
  methods: {
    emitSubmit() {
        console.log("here");
        fetch("http://localhost:3000/api/addUser", {
            method: "post",
            headers: {
            "Content-Type": "application/json"
            },
            body: JSON.stringify({name: this.userData[0], username: this.userData[1], email: this.userData[2], password: this.userData[3], role: this.userData[4]})
        })
        .then(response => response.json())
        .then(() => this.$emit("registerClicked"))
        .catch(err => console.log("Error:", err));
    }
  }
}
</script>