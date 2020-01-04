<template>
  <v-app id="inspire">
    <v-app-bar color="#c12f93" app clipped-left>
      <v-toolbar-title>
        <v-img
          src="https://cdn.ritekit.com/assets/img/ritetag/logo.svg"
          max-width="190"
          max-height="70"
        ></v-img>
      </v-toolbar-title>
    </v-app-bar>

    <v-content color="white">
      <v-container class="fill-height" fluid>
        <v-row align="center" justify="center"><h1 class="display-4">RiteTag</h1></v-row>
        
        <blockquote
          class="mx-auto blockquote"
        >Schedule remarkable post with instant hashtags, emojis, GIFs and CTAs.</blockquote>
        <v-row align="center" justify="center">
          <v-col>
            <v-card class="mx-auto" max-width="400">
              <v-toolbar flat color="#c12f93" dark>
                <v-toolbar-title>Submit a post to twitter</v-toolbar-title>
              </v-toolbar>

              <v-form ref="form">
                <v-alert v-if="failedRequest" class="ma-3 md-3" dense outlined type="error">
                  {{ errorMessage }}
                  <strong>type</strong> of info
                </v-alert>
                <v-alert v-else color="#c12f93" class="ma-3 md-3" dense type="info">
                  {{ errorMessage }}
                  when ready press post to see the
                  <strong>magic</strong>
                </v-alert>
                <v-textarea
                  class="pa-md-3"
                  v-model="postData"
                  filled
                  color="white"
                  label="Prepare your twitter post for the world"
                  value="Hello my name is Rashad Madison and I am looking for work as a frontend developer"
                  :rules="inputRules"
                ></v-textarea>
              </v-form>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn @click="submit" :loading="loading" color="#c12f93" depressed>
                  <v-icon>mdi-auto-fix</v-icon>Post
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-col>
          <v-col>
            <v-card class="mx-auto" color="#26c6da" dark max-width="400">
              <v-card-title>
                <v-icon large left>mdi-twitter</v-icon>
                <span class="title font-weight-light">Twitter</span>
              </v-card-title>

              <v-card-text
                v-if="returnedData.post"
                class="headline font-weight-bold"
              >{{returnedData.post}}</v-card-text>
              <v-card-text v-else class="headline font-weight-bold">{{postData}}</v-card-text>

              <v-card-actions>
                <v-list-item class="grow">
                  <v-list-item-avatar color="grey darken-3">
                    <v-img class="elevation-6" :src="profileImg"></v-img>
                  </v-list-item-avatar>

                  <v-list-item-content>
                    <v-list-item-title>{{ userName }}</v-list-item-title>
                  </v-list-item-content>

                  <v-row align="center" justify="end">
                    <v-icon class="mr-1">mdi-heart</v-icon>
                    <span class="subheading mr-2">256</span>
                    <span class="mr-1">·</span>
                    <v-icon class="mr-1">mdi-share-variant</v-icon>
                    <span class="subheading">45</span>
                  </v-row>
                </v-list-item>
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-content>

    <v-footer app>
      <span>&copy;</span>
    </v-footer>
  </v-app>
</template>

<script>
import axios from "axios";
require("dotenv").config();

export default {
  props: {
    source: String
  },
  data: () => ({
    drawer: null,
    postData:
      "Hello my name is Rashad Madison and at the moment I am looking for work as a frontend developer. ",
    profileImg:
      "https://avataaars.io/?avatarStyle=Transparent&topType=ShortHairShortCurly&accessoriesType=Prescription02&hairColor=Black&facialHairType=Blank&clotheType=Hoodie&clotheColor=White&eyeType=Default&eyebrowType=DefaultNatural&mouthType=Default&skinColor=Light",
    userName: "John Doe",
    returnedData: "",
    clientID: "&client_id=" + process.env.VUE_APP_Client_ID,
    clientSecret: process.env.VUE_APP_Client_Secret,
    inputRules: [v => v.length >= 3 || "Minimum length is 3 characters"],
    loading: false,
    failedRequest: false,
    errorMessage: ""
  }),
  methods: {
    submit() {
      if (this.$refs.form.validate()) {
        this.loading = true;
        axios
          .post(
            "https://api.ritekit.com/v1/stats/auto-hashtag?post=" +
              this.postData +
              this.clientID
          )
          .then(
            response => (
              (this.returnedData = response.data), (this.loading = false)
            )
          )
          .catch(
            error => (
              (this.errorMessage = error),
              (this.failedRequest = true),
              (this.loading = false)
            )
          );
      }
    }
  },
  created() {
    this.$vuetify.theme.dark = true;
  }
};
</script>