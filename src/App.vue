<template>
  <v-app>
    <h1 class="display-3 mt-10 text-center" style="font-family: sans-serif !important;">NASA Social Badge</h1>
    <v-container fluid>
      <v-layout align-center justify-center>
        <v-form>
          <v-container>
            <v-row>
              <v-text-field
                class="mt-3"
                label="Name"
                v-model="name"
                @change="checkEmpty()"
                :rules="nameRules"
                required
                prepend-inner-icon="mdi-account"
              ></v-text-field>
            </v-row>
            <v-row>
              <v-text-field
                class="mt-3"
                label="Twitter Handle"
                v-model="twitter_username"
                prepend-inner-icon="mdi-twitter"
              ></v-text-field>
            </v-row>
            <v-row>
              <v-text-field
                class="mt-3 mb-5"
                label="Instagram Handle"
                v-model="instagram_username"
                prepend-inner-icon="mdi-instagram"
              ></v-text-field>
            </v-row>
            <v-row>
              <v-btn
                class="align-center justify-center"
                color="primary"
                :disabled="!isValid"
                :style="{ left: '50%', transform: 'translateX(-50%)' }"
                @click="submit"
                >Generate</v-btn
              >
            </v-row>
            <v-row>
              <v-btn
                class="align-center justify-center mt-3"
                color="success"
                :disabled="!imageAvailable"
                :style="{ left: '50%', transform: 'translateX(-50%)' }"
                @click="downloadImage"
                >Download</v-btn
              >
            </v-row>
          </v-container>
        </v-form>
      </v-layout>
    </v-container>
  </v-app>
</template>

<script>
import axios from "axios";

export default {
  name: "App",

  data() {
    return {
      isValid: false,
      imageAvailable: false,
      imageURL: "",
      name: null,
      nameRules: [(v) => !!v || "Name is required"],
      twitter_username: null,
      instagram_username: null,
    };
  },
  methods: {
    checkEmpty() {
      if (this.name.length > 0) {
        this.isValid = true;
      } else {
        this.isValid = false;
      }
    },
    submit() {
      axios
        .post(`https://cvdl.ai/nsb/`, {
          name: this.name,
          twitter_username: this.twitter_username,
          instagram_username: this.instagram_username,
        })
        .then((response) => {
          this.imageAvailable = true;
          this.imageURL =
            "https://cvdl.ai/media/" + response.data.badge + ".jpg";
        })
        .catch((e) => {
          console.log(e);
        });
    },
    forceFileDownload(response) {
      const url = window.URL.createObjectURL(new Blob([response.data]));
      const link = document.createElement("a");
      link.href = url;
      link.setAttribute("download", "badge.jpg");
      document.body.appendChild(link);
      link.click();
    },
    downloadImage() {
      axios({
        method: "get",
        url: this.imageURL,
        responseType: "arraybuffer",
      })
        .then((response) => {
          this.forceFileDownload(response);
        })
        .catch((e) => console.log(e));
    },
  },
};
</script>

<style scoped>
.v-text-field {
  width: 300px;
}
</style>
