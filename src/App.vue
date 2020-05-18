<template>
  <v-app>
    <v-container fill-height fluid>
      <v-layout align-center justify-center>
        <v-form>
          <v-container>
            <v-row>
              <v-col>
                <v-text-field
                  label="Name"
                  prepend-inner-icon="mdi-account"
                ></v-text-field>
              </v-col>

              <v-col>
                <v-text-field
                  label="Twitter Handle"
                  prepend-inner-icon="mdi-twitter"
                ></v-text-field>
              </v-col>

              <v-col>
                <v-text-field
                  label="Instagram Handle"
                  prepend-inner-icon="mdi-instagram"
                ></v-text-field>
              </v-col>
            </v-row>
          </v-container>
        </v-form>
      </v-layout>
    </v-container>
  </v-app>
</template>

<script>
export default {
  name: "App",

  components: {},

  data: () => ({
    //
  }),
};
</script>

<style scoped>
.v-text-field {
  width: 200px;
}
</style>
