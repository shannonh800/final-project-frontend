<style>
  @import url('https://fonts.googleapis.com/css2?family=Inter&display=swap');
  html, body {font-family: "Inter", sans-serif;}
  @import "./index.css";
</style>

<template>
  <body>
    <!--Basic setup for the website (app title, description, layout)-->
    <div class="grid grid-cols-9 gap-4">
      <!--Search button-->
      <div class="md:col-span-3 pl-6 ml-6 pt-0 mt-0 text-left align-text-top mb-2 font-bold text-4xl leading-none dark:text-white">A Foodie's Guide</div>
      <input v-model="searchName" type="search" class="md:col-start-5 mr-0 pr-0 col-span-2 form-control relative flex-auto min-w-0 block w-full px-3 py-1.5 text-base font-normal text-gray-700 bg-white bg-clip-padding border border-solid border-gray-300 rounded transition ease-in-out m-0 focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none" placeholder="Search" aria-label="Search" aria-describedby="button-addon2">
      <button @click="getFilteredRestaurants" class="md:col-start-7 max-w- btn inline-block px-4 mr-20 py-2.5 bg-blue-600 text-white font-medium text-xs leading-tight uppercase rounded shadow-md hover:bg-blue-700 hover:shadow-lg focus:bg-blue-700  focus:shadow-lg focus:outline-none focus:ring-0 active:bg-blue-800 active:shadow-lg transition duration-150 ease-in-out items-center" type="button" id="button-addon2">
        <svg aria-hidden="true" focusable="false" data-prefix="fas" data-icon="search" class="w-4" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
          <path fill="currentColor" d="M505 442.7L405.3 343c-4.5-4.5-10.6-7-17-7H372c27.6-35.3 44-79.7 44-128C416 93.1 322.9 0 208 0S0 93.1 0 208s93.1 208 208 208c48.3 0 92.7-16.4 128-44v16.3c0 6.4 2.5 12.5 7 17l99.7 99.7c9.4 9.4 24.6 9.4 33.9 0l28.3-28.3c9.4-9.4 9.4-24.6.1-34zM208 336c-70.7 0-128-57.2-128-128 0-70.7 57.2-128 128-128 70.7 0 128 57.2 128 128 0 70.7-57.2 128-128 128z"></path>
        </svg>
      </button>
    </div>
    <div class="md:grid md:grid-cols-3 md:gap-6">
      <div class="md:col-span-1">
        <div class="px-4 sm:px-0">
          <h3 class="pl-6 ml-6 pt-6 mt-0 text-left mb-2 text-xl font-medium leading-6 text-gray-900">Description</h3>
          <p class="mt-1 leading-6 pl-6 ml-6 mb-4 text-sm text-left text-gray-600">
            Living in New York City means countless food options, which is ideal for the everyday foodie but perhaps not ideal for the indecisive and risk-averse. 
            Without knowing what a restaurant sells or if its food is any good, making a decision on where to go for dinner can be a challenge, but that's where 
            A Foodie's Guide comes in!
          </p>
          <p class="mt-2 pl-6 ml-6 leading-6 text-sm text-left text-gray-600">
            A Foodie's Guide is a web app that allows users to browse and search for restaurants by name in NYC and see its details in terms of what it sells, contact info, etc., 
            but most importantly, other users' reviews of it. Users are able to add restaurants to the app.
          </p>
        </div>
      </div>
      <div class="mt-5 pt-4 md:col-span-2 col-start-2 md:mt-0">
        <button v-show="adminRole"
          class="text-left mr-0 h-fit mt-1 bg-blue-600 text-white border hover:border-blue-600 hover:text-blue-600 hover:bg-white px-8 py-2 text-sm uppercase tracking-wide font-bold rounded-lg"
          @click="openRestModal">
          Add Restaurant
        </button>
        <ModalTemplate :showing="(restModalShowing && restNotSubmitted)" @close="closeRestModal">
          <RestaurantForm v-show="restNotSubmitted" @restSubmitted="hideFormAddRest"/>
        </ModalTemplate>
        <RestaurantDetail :getData="getData" :restName="chosenRestaurant" v-show="showRestDetail"/>

        <div v-for="restaurant in restaurants" v-bind:key="restaurant"><RestaurantCard :restaurant="restaurant" v-show="restNotClicked" @restClicked="hideRestCardAndDisplayDetails(restaurant.name)"/></div>
        
        <br>
      </div>
    </div>
  </body>
</template>

<script>
import RestaurantCard from "./components/RestaurantCard.vue";
import RestaurantForm from "./components/RestaurantForm.vue";
import ModalTemplate from "./components/ModalTemplate.vue";
import RestaurantDetail from "./components/RestaurantDetail.vue";

export default {
  name: 'App',
  components: {
    RestaurantCard,
    RestaurantForm,
    ModalTemplate,
    RestaurantDetail
  },
  data() {
    return {
      errorMessage: "Test Message",
      restaurants: [],
      restNotClicked: true,
      restModalShowing: false,
      restNotSubmitted: true,
      adminRole: true,
      showRestDetail: false,
      chosenRestaurant: "",
      getData: false,
      searchName: ""
    }
  },
  methods: {
    loadAllRestaurants() {
      fetch("http://localhost:3000/api/restaurants", {mode: "cors"})
        .then(response => response.json())
        .then(data => {
          this.restaurants = data;

          this.restaurants.map(restaurant => {
            if(isNaN(restaurant.rating)) {
                restaurant.rating = "No Rating Yet";
              }
          })
        });
    },
    hideRestCardAndDisplayDetails(restaurantName) {
      this.restNotClicked = false;
      this.showRestDetail = true;
      this.chosenRestaurant = restaurantName;
      this.getData = true;
    },
    hideFormAddRest() {
      this.restNotSubmitted = false;
      this.loadAllRestaurants();
    },
    openRestModal() {
      this.restModalShowing = true;
      this.restNotSubmitted = true;
    },
    closeRestModal() {
      this.restModalShowing = false;
      this.restNotSubmitted = false;
    },
    getFilteredRestaurants() {
      fetch("http://localhost:3000/api/restaurants", {mode: "cors"})
        .then(response => response.json())
        .then(data => {
          this.restaurants = data.filter(restaurant => {
            return restaurant.name == this.searchName;
          });
        });
    }
  },
  mounted() {
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
