<template>
  <div>
    <div class="text-left max-w-fit max-h-fit ml-24 pt-4 pb-6 px-4">
          <div class="pt-5 pb-0 pl-14 pr-60">
            <p class='font-bold text-2xl mb-3 leading-3'>{{restDetails.name}}</p>
            <div class="flex flex-row items-center">
              <p class="underline text-sm text-grey font-bold  mr-1 mt-1 leading-2">{{restDetails.rating}}</p>
              <svg aria-hidden="true" class="mt-1 leading-2 w-5 h-5 text-yellow-400" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><title>Rating star</title><path d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"></path></svg>
            </div>
            <p class="text-sm text-black block mt-6 leading-3">Address: {{restDetails.address}}</p>
            <p class="text-sm text-black block mt-6 leading-3">Website: {{restDetails.website}}</p>
            <p class="text-sm text-black block mt-6 leading-3">Phone: {{restDetails.phone}}</p>
            <p class="text-sm text-black block mt-6 leading-3">Type of Food: {{restDetails.keywords}}</p>
          </div>
    </div>

    <p class='text-left ml-40 pl-2 font-bold underline text-lg mb-3 mt-4 leading-3'>Reviews:</p>

    <p v-show="hasNoReviews" class='text-left ml-40 pl-2 font-bold text-sm mb-3 mt-4 leading-3'>There are no reviews yet</p>
    
    <div class="text-left max-w-fit max-h-fit ml-24 pt-4 pb-6 px-4">
      <div v-for="review in restDetails.reviews" v-bind:key="review"><ReviewCard :review="review"/></div>
    </div>
    <div class="text-left max-w-fit max-h-fit ml-36 mx-auto px-4">
      <button
        class="h-fit mt-1 bg-blue-600 text-white border hover:border-blue-600 hover:text-blue-600 hover:bg-white px-8 py-2 text-sm uppercase tracking-wide font-bold rounded-lg"
        @click="openReviewModal">
        Add A Review
      </button>
      <ModalTemplate :showing="(reviewModalShowing & reviewNotSubmitted)" @close="closeReviewModal">
        <ReviewForm :restName="restDetails.name" v-show="reviewNotSubmitted" @reviewSubmitted="hideFormAddReview"/>
      </ModalTemplate>
    </div>
  </div>

  </template>
  
  <script>
  import ReviewCard from "./ReviewCard.vue";
  import ReviewForm from "./ReviewForm.vue";
  import ModalTemplate from "./ModalTemplate.vue";

  export default {
    name: 'RestaurantDetail',
    data() {
      return {
        reviewModalShowing: false,
        reviewNotSubmitted: true,
        restDetails: {name: "", address: "", rating: "", website: "", phone: "", keywords: "", reviews: ""},
        hasNoReviews: false
      }
    },
    props: {
      restName: {
        required: true,
        type: String
      },
      getData: {
        required: true,
        type: Boolean
      }
    },
    components: {
    ReviewForm,
    ModalTemplate,
    ReviewCard
},
    methods: {
      getRestaurantInfo() {
        if(this.getData === true) {
          fetch("http://localhost:3000/api/restaurantDetails", {
            method: "post",
            headers: {
              "Content-Type": "application/json"
            },
            body: JSON.stringify({restName: this.restName})
          })
          .then(response => response.json())
          .then(data => {
            // receives array of restaurants w/ restaurant name
            if(data[0] !== undefined) {
              this.restDetails = data[0];
              if(isNaN(this.restDetails.rating)) {
                this.restDetails.rating = "No Rating Yet";
                this.hasNoReviews = true;
              }
            }
          });
        }
      },
      loadAllReviews() {
        if(this.restDetails.name !== "") {
          fetch("http://localhost:3000/api/reviews", {mode: "cors"})
          .then(response => response.json())
          .then(data => {
            this.reviews = data;
          });
        }
      },
      hideFormAddReview() {
        this.reviewNotSubmitted = false;
        this.loadAllReviews();
      },
      openReviewModal() {
      this.reviewModalShowing = true;
      this.reviewNotSubmitted = true;
      },
      closeReviewModal() {
        this.reviewModalShowing = false;
        this.reviewNotSubmitted = false;
      }
    },
    beforeUpdate() {
      //this.loadAllReviews()
      // include function that gets the restaurant data based on the restName passed in
      this.getRestaurantInfo()
    }
  }
  </script>