<style>
  @import url('https://fonts.googleapis.com/css2?family=Inter&display=swap');
  html, body {font-family: "Inter", sans-serif;}
  @import "./index.css";
</style>

<template>
  <body>
    <!--Basic setup for the website (app title, description, layout)-->
    <div class="grid grid-cols-9 gap-4">
      <div class="md:col-span-3 pl-6 ml-6 pt-0 mt-0 text-left align-text-top mb-2 font-bold text-4xl leading-none dark:text-white">A Foodie's Guide</div>
      <button v-show="loginNotSubmitted"
        class="md:col-start-7 col-span-1 h-fit mt-1 bg-blue-600 text-white border hover:border-blue-600 hover:text-blue-600 hover:bg-white px-8 text-center py-2 text-sm uppercase tracking-wide font-bold rounded-lg"
        @click="openLoginModal"
      >
      Log In
      </button>
      <ModalTemplate :showing="(loginModalShowing & loginNotSubmitted)" @close="closeLoginModal">
        <LoginForm v-show="loginNotSubmitted" @loginClicked="hideLoginAndSignIn"/>
      </ModalTemplate>
      <button v-show="loginNotSubmitted"
        class="md:col-span-1 h-fit mt-1 bg-blue-600 text-white border hover:border-blue-600 hover:text-blue-600 hover:bg-white px-8 py-2 text-sm uppercase tracking-wide font-bold rounded-lg"
        @click="openRegisterModal"
      >
      Sign Up
      </button>
      <ModalTemplate :showing="(registerModalShowing & registerNotSubmitted)" @close="closeRegisterModal">
        <RegisterForm v-show="registerNotSubmitted" @registerClicked="hideRegisterForm"/>
      </ModalTemplate>
    </div>
    <div class="md:grid md:grid-cols-3 md:gap-6">
      <div class="md:col-span-1">
        <div class="px-4 sm:px-0">
          <h3 class="pl-6 ml-6 pt-6 mt-0 text-left mb-2 text-lg font-medium leading-6 text-gray-900">Description</h3>
          <p class="mt-1 pl-6 ml-6 text-sm text-left text-gray-600">
            Living in New York City means countless food options, which is ideal for the everyday foodie but perhaps not ideal for the indecisive and risk-averse. 
            Without knowing what a restaurant sells or if its food is any good, making a decision on where to go for dinner can be a challenge, but that's where 
            A Foodie's Guide comes in!
          </p>
          <p class="mt-2 pl-6 ml-6 text-sm text-left text-gray-600">
            A Foodie's Guide is a web app that allows users to search up a restaurant in NYC and see its details in terms of what it sells, its menu, hours, etc., 
            but most importantly, other users' reviews of it. It also provides a map function, so users can bookmark their favorite spots or places they've 
            been meaning to go to later and visualize it in a more convenient manner--useful when trying to decide on a spot in a specific area.
          </p>
        </div>
      </div>
      <div class="mt-5 pt-4 md:col-span-2 md:mt-0">
        <button v-show="adminRole"
          class="h-fit mt-1 bg-blue-600 text-white border hover:border-blue-600 hover:text-blue-600 hover:bg-white px-8 py-2 text-sm uppercase tracking-wide font-bold rounded-lg"
          @click="openRestModal">
          Add Restaurant
        </button>
        <ModalTemplate :showing="(restModalShowing & restNotSubmitted)" @close="closeRestModal">
          <RestaurantForm v-show="restNotSubmitted" @restSubmitted="hideFormAddRest"/>
        </ModalTemplate>
        <RestaurantDetail :restName="chosenRestaurant" v-show="showRestDetail"/>

        <div v-for="restaurant in restaurants" v-bind:key="restaurant"><RestaurantCard :restaurant="restaurant" v-show="restNotClicked" @restClicked="hideRestCardAndDisplayDetails(restaurant.name)"/></div>
        
        <br>
        <ReviewCard v-show="reviewAdded"/>
        <br>
      </div>
    </div>
  </body>
</template>

<script>
import LoginForm from "./components/LoginForm.vue";
import RestaurantCard from "./components/RestaurantCard.vue";
import RestaurantForm from "./components/RestaurantForm.vue";
import ReviewCard from "./components/ReviewCard.vue";
import RegisterForm from "./components/RegisterForm.vue";
import ModalTemplate from "./components/ModalTemplate.vue";
import RestaurantDetail from "./components/RestaurantDetail.vue";

export default {
  name: 'App',
  components: {
    LoginForm,
    RestaurantCard,
    RestaurantForm,
    ReviewCard,
    RegisterForm,
    ModalTemplate,
    RestaurantDetail
  },
  data() {
    return {
      errorMessage: "Test Message",
      restaurants: [],
      restNotClicked: true,
      reviewAdded: false,
      // ^^ may not need reviewAdded
      loginModalShowing: false,
      loginNotSubmitted: true,
      registerModalShowing: false,
      registerNotSubmitted: true,
      restModalShowing: false,
      restNotSubmitted: true,
      adminRole: true,
      showRestDetail: false,
      chosenRestaurant: ""
    }
  },
  methods: {
    checkRole() {
      fetch("http://localhost:3000/api/role", {mode: "cors"})
        .then(response => response.json())
        .then(data => {
          console.log(data);
          this.adminRole = data.adminRole;
        });
    },
    loadAllRestaurants() {
      fetch("http://localhost:3000/api/restaurants", {mode: "cors"})
        .then(response => response.json())
        .then(data => {
          console.log(data);
          this.restaurants = data;
        });
    },
    hideLoginAndSignIn() {
      this.loginNotSubmitted = false;
      this.checkRole();
      console.log(this.adminRole);
    },
    hideRegisterForm() {
      this.registerNotSubmitted = false;
    },
    hideRestCardAndDisplayDetails(restaurantName) {
      console.log(restaurantName);
      this.restNotClicked = false;
      this.showRestDetail = true;
      this.chosenRestaurant = restaurantName;
    },
    showReview() {
      this.reviewAdded = true;
    },
    hideFormAddRest() {
      this.restNotSubmitted = false;
      this.loadAllRestaurants();
    },
    openLoginModal() {
      this.loginModalShowing = true;
      this.loginNotSubmitted = true;
    },
    closeLoginModal() {
      this.loginModalShowing = false;
      this.loginNotSubmitted = false;
    },
    openRegisterModal() {
      this.registerModalShowing = true;
      this.registerNotSubmitted = true;
    },
    closeRegisterModal() {
      this.registerModalShowing = false;
      this.registerNotSubmitted = false;
    },
    openRestModal() {
      this.restModalShowing = true;
      this.restNotSubmitted = true;
    },
    closeRestModal() {
      this.restModalShowing = false;
      this.restNotSubmitted = false;
    }
  },
  mounted() {
    //this.checkRole();
    this.loadAllRestaurants();
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
