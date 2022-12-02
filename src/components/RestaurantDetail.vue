<template>
  <div>
    <img alt='' class='ml-40 w-80 h-40 mt-4 block rounded-lg' src='https://i.pinimg.com/736x/aa/c0/33/aac033eef0a730db2fb7168bc107931e.jpg'>
    <div class="text-left max-w-fit max-h-fit ml-24 pt-4 pb-6 px-4">
          <div class="pt-5 pb-0 pl-14 pr-60">
            <p class='font-bold text-lg mb-3 leading-3'>{{restName}}</p>
            <p class="text-sm text-grey font-bold underline block mt-6 leading-3">{insert rating + star svg}</p>
            <p class="text-sm text-black block mt-6 leading-3">Address:</p>
            <p class="text-sm text-black block mt-6 leading-3">Type of Food:</p>
          </div>
    </div>
    <div class="text-left max-w-fit max-h-fit ml-36 mx-auto px-4">
      <button
        class="h-fit mt-1 bg-blue-600 text-white border hover:border-blue-600 hover:text-blue-600 hover:bg-white px-8 py-2 text-sm uppercase tracking-wide font-bold rounded-lg"
        @click="openReviewModal">
        Add A Review
      </button>
      <ModalTemplate :showing="(reviewModalShowing & reviewNotSubmitted)" @close="closeReviewModal">
        <ReviewForm v-show="reviewNotSubmitted" @reviewSubmitted="hideFormAddReview"/>
      </ModalTemplate>
    </div>
  </div>

  </template>
  
  <script>
  import ReviewForm from "./ReviewForm.vue";
  import ModalTemplate from "./ModalTemplate.vue";

  export default {
    name: 'RestaurantDetail',
    data() {
      return {
        reviewModalShowing: false,
        reviewNotSubmitted: true,
        restDetails: null
      }
    },
    props: {
      restName: {
        required: true,
        type: String
      }
    },
    components: {
      ReviewForm,
      ModalTemplate
    },
    methods: {
      getRestaurantInfo() {
        console.log("Restaurant Name Here: ", this.restName);
        fetch("http://localhost:3000/api/restaurantDetails", {
          method: "post",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify({restName: this.restName})
        })
        .then(response => response.json())
        .then(data => {
          console.log(data);
          console.log(Object.keys(data));
          this.restDetails = data;
          console.log(this.restDetails.name);
        });
      },
      loadAllReviews() {
        // FIX THIS STATEMENT TO ACTUALLY GET THE REVIEWS!!
        fetch("http://localhost:3000/api/reviews", {mode: "cors"})
        .then(response => response.json())
        .then(data => {
          console.log(data);
          this.reviews = data;
        });
      },
      hideFormAddReview() {
        this.reviewNotSubmitted = false;
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