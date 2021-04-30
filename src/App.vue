<template>
  <div id="app">
    <modal name="post-modal" adaptive>
      <input
        type="text"
        id="post-username"
        placeholder="Όνομα"
        @input="checkInput"
        v-model="username"
        maxlength="30"
      />
      <textarea
        id="post-input"
        cols="30"
        rows="7"
        maxlength="400"
        placeholder="Κείμενο"
        @input="checkInput"
        v-model="postContent"
      ></textarea>
      <br />
      <button id="submit-button" :disabled="buttonState" @click="createPost">
        {{ buttonText }}
      </button>
    </modal>

    <h1>Ανομολόγητα 1ου Γυμνασίου Νεάπολης</h1>
    <span
      >Design by
      <a class="link" target="_blank" href="https://www.instagram.com/tasos.js/"
        >Tasos</a
      >, API by
      <a
        class="link"
        target="_blank"
        href="https://www.instagram.com/pasenidiss/"
        >Edward</a
      >
      &
      <a
        class="link"
        target="_blank"
        href="https://www.instagram.com/_.kalogeropoylos/"
        >Alkis</a
      ></span
    >
    <br />

    <button id="post-button" @click="show">
      Γραψε κάτι που σε απασχολεί 📋
    </button>

    <br />
    <span id="alert">{{ this.alertMessage }}</span>
    <div class="post-container">
      <div v-for="i in postData" :key="i.id">
        <Post :info="i" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Post from "./components/Post.vue";

export default {
  name: "App",
  components: {
    Post,
  },
  methods: {
    getData() {
      this.alertMessage = "Φόρτωση...";
      axios.get("https://1gym.tk/api/posts").then((response) => {
        if (response.status == 200) {
          if (response.data.length > 0) {
            this.postData = response.data;
            this.alertMessage = "";
          } else {
            this.alertMessage = "Δεν βρέθηκαν ανομολόγητα 😔";
          }
        } else {
          this.alertMessage = "Δεν βρέθηκαν ανομολόγητα 😔";
        }
      });
    },
    createPost() {
      if (this.timer) {
        this.buttonText = "Ανέβασμα...";
        const formData = new FormData();
        formData.append("username", this.username);
        formData.append("confession", this.postContent);
        axios.post("https://1gym.tk/api/post", formData).then((response) => {
          if (response.status == 200) {
            this.$modal.hide("post-modal");
            this.getData();
            this.buttonText = "Στείλ'το ✉️";
            this.timer = false;
            setTimeout(() => {
              this.buttonText = "Στείλ'το ✉️";
              this.timer = true;
              this.checkInput();
            }, 60000);
          } else {
            this.buttonText = "Κάτι πήγε στραβά 😔";
            setTimeout(() => {
              this.buttonText = "Στείλ'το ✉️";
            }, 4000);
          }
        });
      } else {
        this.buttonText = "Περίμενε ένα λεπτό ⏰";
      }
    },
    checkInput() {
      if (this.timer) {
        if (
          this.username == "" ||
          !this.username ||
          this.postContent == "" ||
          !this.postContent
        ) {
          this.buttonState = true;
        } else {
          this.buttonState = false;
        }
      } else {
        this.buttonState = false;
      }
    },
    show() {
      this.$modal.show("post-modal");
    },
  },
  mounted() {
    this.checkInput();
    this.getData();
  },
  data() {
    return {
      timer: true,
      username: null,
      postContent: null,
      buttonState: false,
      buttonText: "Στείλ'το ✉️",
      alertMessage: null,
      postData: null,
    };
  },
};
</script>

<style>
*:focus {
  outline: none;
}

::placeholder {
  color: #ffffff;
  opacity: 0.5;
}

body {
  background-color: #eeeeee;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 2000 1500'%3E%3Cdefs%3E%3CradialGradient id='a' gradientUnits='objectBoundingBox'%3E%3Cstop offset='0' stop-color='%23b5b5b5'/%3E%3Cstop offset='1' stop-color='%23eeeeee'/%3E%3C/radialGradient%3E%3ClinearGradient id='b' gradientUnits='userSpaceOnUse' x1='0' y1='750' x2='1550' y2='750'%3E%3Cstop offset='0' stop-color='%23d2d2d2'/%3E%3Cstop offset='1' stop-color='%23eeeeee'/%3E%3C/linearGradient%3E%3Cpath id='s' fill='url(%23b)' d='M1549.2 51.6c-5.4 99.1-20.2 197.6-44.2 293.6c-24.1 96-57.4 189.4-99.3 278.6c-41.9 89.2-92.4 174.1-150.3 253.3c-58 79.2-123.4 152.6-195.1 219c-71.7 66.4-149.6 125.8-232.2 177.2c-82.7 51.4-170.1 94.7-260.7 129.1c-90.6 34.4-184.4 60-279.5 76.3C192.6 1495 96.1 1502 0 1500c96.1-2.1 191.8-13.3 285.4-33.6c93.6-20.2 185-49.5 272.5-87.2c87.6-37.7 171.3-83.8 249.6-137.3c78.4-53.5 151.5-114.5 217.9-181.7c66.5-67.2 126.4-140.7 178.6-218.9c52.3-78.3 96.9-161.4 133-247.9c36.1-86.5 63.8-176.2 82.6-267.6c18.8-91.4 28.6-184.4 29.6-277.4c0.3-27.6 23.2-48.7 50.8-48.4s49.5 21.8 49.2 49.5c0 0.7 0 1.3-0.1 2L1549.2 51.6z'/%3E%3Cg id='g'%3E%3Cuse href='%23s' transform='scale(0.12) rotate(60)'/%3E%3Cuse href='%23s' transform='scale(0.2) rotate(10)'/%3E%3Cuse href='%23s' transform='scale(0.25) rotate(40)'/%3E%3Cuse href='%23s' transform='scale(0.3) rotate(-20)'/%3E%3Cuse href='%23s' transform='scale(0.4) rotate(-30)'/%3E%3Cuse href='%23s' transform='scale(0.5) rotate(20)'/%3E%3Cuse href='%23s' transform='scale(0.6) rotate(60)'/%3E%3Cuse href='%23s' transform='scale(0.7) rotate(10)'/%3E%3Cuse href='%23s' transform='scale(0.835) rotate(-40)'/%3E%3Cuse href='%23s' transform='scale(0.9) rotate(40)'/%3E%3Cuse href='%23s' transform='scale(1.05) rotate(25)'/%3E%3Cuse href='%23s' transform='scale(1.2) rotate(8)'/%3E%3Cuse href='%23s' transform='scale(1.333) rotate(-60)'/%3E%3Cuse href='%23s' transform='scale(1.45) rotate(-30)'/%3E%3Cuse href='%23s' transform='scale(1.6) rotate(10)'/%3E%3C/g%3E%3C/defs%3E%3Cg transform='rotate(0 0 0)'%3E%3Cg transform='rotate(0 0 0)'%3E%3Ccircle fill='url(%23a)' r='3000'/%3E%3Cg opacity='0.5'%3E%3Ccircle fill='url(%23a)' r='2000'/%3E%3Ccircle fill='url(%23a)' r='1800'/%3E%3Ccircle fill='url(%23a)' r='1700'/%3E%3Ccircle fill='url(%23a)' r='1651'/%3E%3Ccircle fill='url(%23a)' r='1450'/%3E%3Ccircle fill='url(%23a)' r='1250'/%3E%3Ccircle fill='url(%23a)' r='1175'/%3E%3Ccircle fill='url(%23a)' r='900'/%3E%3Ccircle fill='url(%23a)' r='750'/%3E%3Ccircle fill='url(%23a)' r='500'/%3E%3Ccircle fill='url(%23a)' r='380'/%3E%3Ccircle fill='url(%23a)' r='250'/%3E%3C/g%3E%3Cg transform='rotate(0 0 0)'%3E%3Cuse href='%23g' transform='rotate(10)'/%3E%3Cuse href='%23g' transform='rotate(120)'/%3E%3Cuse href='%23g' transform='rotate(240)'/%3E%3C/g%3E%3Ccircle fill-opacity='0.1' fill='url(%23a)' r='3000'/%3E%3C/g%3E%3C/g%3E%3C/svg%3E");
  background-attachment: fixed;
  background-size: cover;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 2em;
}

.post-container {
  max-width: 1200px;
  margin: 0 auto;
  display: grid;
  grid-gap: 1rem;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
}

.post {
  overflow-wrap: break-word;
  color: #ffffff;
  font-size: 1.3em;
  background-color: #000000;
  border-radius: 25px;
  padding: 0.2em 0.8em;
  text-align: left;
}

#post-button {
  transition-duration: 0.4s;
  font-size: 1.3em;
  border: none;
  border-radius: 25px;
  margin: 1em 1em;
  padding: 0.6em 1em;
  color: #ffffff;
  background-color: #000000;
}

#post-button:hover {
  cursor: pointer;
  transform: translateY(-3px);
  box-shadow: 1px 8px 21px -4px rgba(0, 0, 0, 0.75);
}

#post-button:active {
  transform: translateY(0px);
}

#alert {
  font-size: 1.2em;
}

.link {
  color: #02b12e;
}

.vm--modal {
  border-radius: 30px !important;
  background-color: #000000 !important;
}

#post-username {
  color: #ffffff;
  padding: 0.3em 0.7em;
  height: 1.8em;
  font-size: 1.1em;
  background-color: #2c3e50;
  border: none;
  border-radius: 25px;
  margin-top: 1em;
  width: 90%;
}

#post-input {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #ffffff;
  padding: 0.3em 0.7em;
  font-size: 1.1em;
  background-color: #2c3e50;
  border: none;
  border-radius: 25px;
  margin-top: 0.6em;
  width: 90%;
  resize: none;
}

#submit-button {
  transition-duration: 0.4s;
  font-size: 1.2em;
  border: none;
  border-radius: 25px;
  margin-top: 0.4em;
  padding: 0.4em 0.8em;
  color: #ffffff;
  background-color: rgb(2, 177, 46);
  cursor: pointer;
}

#submit-button:disabled {
  background-color: rgb(178, 179, 178);
  cursor: no-drop;
}

#submit-button:hover {
  box-shadow: 1px 8px 21px -11px rgba(2, 177, 46, 0.75);
  background-color: rgb(3, 194, 50);
}

#submit-button:disabled:hover {
  background-color: rgb(178, 179, 178);
  cursor: no-drop;
}
</style>
