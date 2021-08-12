<template>
  <form @submit.prevent="submitForm">
    <div class="form-control">
      <label for="user-name">Your Name</label>
      <input
        v-model.trim="userName"
        id="user-name"
        name="user-name"
        type="text"
      />
    </div>
    <div class="form-control">
      <label for="age">Your Age (Years)</label>
      <input
        v-model="userAge"
        id="age"
        name="age"
        type="number"
        min="0"
        max="120"
      />
    </div>
    <div class="form-control">
      <label for="referrer">How did you hear about us?</label>
      <select v-model="referrer" id="referrer" name="referrer">
        <option value="google">Google</option>
        <option value="wom">Word of mouth</option>
        <option value="newspaper">Newspaper</option>
      </select>
    </div>
    <div class="form-control">
      <h2>What are you interested in?</h2>
      <div>
        <input
          v-model="interest"
          id="interest-news"
          name="interest"
          type="checkbox"
          value="news"
        />
        <label for="interest-news">News</label>
      </div>
      <div>
        <input
          v-model="interest"
          id="interest-tutorials"
          name="interest"
          type="checkbox"
          value="tutorials"
        />
        <label for="interest-tutorials">Tutorials</label>
      </div>
      <div>
        <input
          v-model="interest"
          id="interest-music"
          name="interest"
          type="checkbox"
          value="music"
        />
        <label for="interest-music">Music</label>
      </div>
    </div>
    <div class="form-control">
      <h2>How do you learn?</h2>
      <div>
        <input
          v-model="howLearn"
          id="how-video"
          name="how"
          type="radio"
          value="videos"
        />
        <label for="how-video">Video Courses</label>
      </div>
      <div>
        <input
          v-model="howLearn"
          id="how-blogs"
          name="how"
          type="radio"
          value="blogs"
        />
        <label for="how-blogs">Blogs</label>
      </div>
      <div>
        <input
          v-model="howLearn"
          id="how-other"
          name="how"
          type="radio"
          value="other"
        />
        <label for="how-other">Other</label>
      </div>
    </div>
    <div class="form-control">
      <h2>How do you rate this app?</h2>
      <rating-control v-model="rating" />
    </div>
    <p v-if="error">{{ error }}</p>
    <div>
      <button>Save Data</button>
    </div>
  </form>
</template>

<script>
import RatingControl from "./RatingControl.vue";

export default {
  components: {
    RatingControl,
  },
  data() {
    return {
      userName: "",
      userAge: null,
      referrer: "google",
      interest: [],
      howLearn: null,
      rating: null,
      error: null,
    };
  },
  emits: ["submit-form"],
  methods: {
    submitForm() {
      console.log(
        "Username: " +
          this.userName +
          ". Age: " +
          this.userAge +
          ". Source of info: " +
          this.referrer +
          ". Interests: " +
          this.interest +
          ". How you learn: " +
          this.howLearn +
          ". Your rating: " +
          this.rating
      );
      const formData = {
        userName: this.userName,
        userAge: this.userAge,
        infoSource: this.referrer,
        interests: this.interest,
        howLearn: this.howLearn,
        rating: this.rating,
      };

      this.error = null;

      fetch(
        "https://vue-form-a6941-default-rtdb.europe-west1.firebasedatabase.app/learning.json",
        {
          method: "POST",
          headers: {
            "Content-type": "appication/json",
          },
          body: JSON.stringify(formData),
        }
      )
        .then((response) => {
          if (
            response.ok &&
            formData.userName &&
            formData.userAge &&
            formData.howLearn &&
            formData.interests &&
            formData.rating
          ) {
            console.log("fetch response is ok");
            alert("Form submitted!");
            this.$emit("submit-form");
            this.clearForm();
          } else {
            throw new Error("Couldn't save data");
          }
        })
        .catch((error) => {
          console.log(error);
          this.error = error.message;
        });
    },
    clearForm() {
      this.userName = "";
      this.userAge = null;
      this.referrer = "google";
      this.interest = [];
      this.howLearn = null;
      this.rating = null;
    },
  },
};
</script>

<style scoped>
form {
  margin: 1rem;
  min-width: 30%;
  max-width: 50%;
  height: max-content;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(3, 43, 2, 0.26);
  padding: 2rem;
  background-color: #ffffff;
}

.form-control {
  margin: 0.5rem 0;
}

label {
  font-weight: bold;
}

h2 {
  font-size: 1rem;
  margin: 0.5rem 0;
}

input,
select {
  display: block;
  width: 100%;
  font: inherit;
  margin-top: 0.5rem;
}

select {
  width: auto;
}

input[type="checkbox"],
input[type="radio"] {
  display: inline-block;
  width: auto;
  margin-right: 1rem;
}

input[type="checkbox"] + label,
input[type="radio"] + label {
  font-weight: normal;
}

button {
  font: inherit;
  border: 1px solid #0076bb;
  background-color: #0076bb;
  color: white;
  cursor: pointer;
  padding: 0.75rem 2rem;
  border-radius: 30px;
}

button:hover,
button:active {
  border-color: #002350;
  background-color: #002350;
}
</style>
