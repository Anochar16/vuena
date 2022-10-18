<template>
  <v-container>
    <div class="d-flex">
      <v-btn depressed color="primary" @click="openHome()"> Back </v-btn>
    </div>

    <v-form>
      <h1 class="d-flex justify-center">Form Customer</h1>
      <v-row>
        <v-col cols="12">
          <v-text-field
            v-model="form.Fullname"
            label="Fullname"
            required
          ></v-text-field>
        </v-col>
        <v-col cols="12">
          <v-text-field
            v-model="form.P_number"
            label="Phone Numberr"
            required
          ></v-text-field>
        </v-col>
        <v-col cols="12" class="d-flex justify-center">
          <v-btn color="success" class="mr-4" @click="submit()"> Submit </v-btn>
        </v-col>
      </v-row>
    </v-form>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      form: {
        Fullname: "",
        P_number: "",
      },
    };
  },
  methods: {
    openHome() {
      this.$router.push({ path: "/" });
    },
    submit() {
      if (this.$route.params.UID) {
        axios
          .put(`http://localhost:3000/customer/${this.$route.params.UID}`, this.form)
          .then((response) => {
            console.log(response);
            this.$router.push({ path: "/" });
          })
          .catch((e) => {
            this.errors.push(e);
          });
      } else {
        axios
          .post(`http://localhost:3000/customer`, this.form)
          .then((response) => {
            console.log(response);
            this.$router.push({ path: "/" });
          })
          .catch((e) => {
            this.errors.push(e);
          });
      }
    },
  },
  mounted() {
    console.log(this.$route.params.UID);
    if (this.$route.params.UID != null) {
      axios
        .get(`http://localhost:3000/customer/${this.$route.params.UID}`)
        .then((response) => {
          this.form = response.data.data;
        })
        .catch((e) => {
          this.errors.push(e);
        });
    }
  },
};
</script>