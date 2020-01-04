<template>
  <v-app id="inspire">
    <v-navigation-drawer v-model="drawer" app clipped>
      <v-list dense>
        <v-list-item link>
          <v-list-item-action>
            <v-icon>mdi-view-dashboard</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Dashboard</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-list-item link>
          <v-list-item-action>
            <v-icon>mdi-settings</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Settings</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar app clipped-left>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-toolbar-title>Application</v-toolbar-title>
    </v-app-bar>

    <v-content>
      <v-container class="fill-height" fluid>
        <v-row align="center" justify="center">
          <v-col>
            <v-card class="mx-auto" max-width="400">
              <v-toolbar flat color="#c12f93" dark>
                <v-toolbar-title>Submit a post to twitter</v-toolbar-title>
              </v-toolbar>

              <v-form ref="form">
                <v-alert v-if="failedRequest" class="ma-md-3" dense outlined type="error">
                  {{ error }}
                  <strong>type</strong> of info
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
                <v-btn color="#c12f93" depressed>
                  <v-icon>mdi-auto-fix</v-icon>Enhance
                </v-btn>
                <v-btn @click="submit" :loading="loading" color="#c12f93" depressed>
                  <v-icon>mdi-twitter</v-icon>Post
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

              <v-card-text class="headline font-weight-bold">{{ postData }}</v-card-text>

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
      <span>&copy; 2020 {{ data }}</span>
    </v-footer>
  </v-app>
</template>

<script>
import axios from 'axios';
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
    data: [],
    clientID: process.env.VUE_APP_Client_ID,
    clientSecret: process.env.VUE_APP_Client_Secret,
    inputRules: [v => v.length >= 3 || "Minimum length is 3 characters"],
    loading: false,
    failedRequest: false,
    errorMessage: ''
  }),
  methods: {
    submit() {
      if (this.$refs.form.validate()) {
        this.loading = true;
        console.log(this.postData);
      }
    }
  },
  created() {
    this.$vuetify.theme.dark = true;
  },
  mounted() {
    axios
      .get(
        "https://api.ritekit.com/v1/stats/multiple-hashtags?tags=php&client_id=" +
          this.clientID
      )
      .then(function(response) {
        this.data = response;
      })
      .catch(function(error) {
        this.errorMessage = error;
      })
      .finally(function() {
        // always executed
        console.log(this.data);
      });
  }
};
</script>