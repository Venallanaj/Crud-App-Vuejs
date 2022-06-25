<template>
  <b-form class="mt-3">
    <b-row>
      <b-col cols="6">
        <b-form-group id="name" label="Full Name" label-for="name">
          <b-form-input
            id="name"
            type="text"
            placeholder="John Gonzales"
            v-model="user.name"
          >
          </b-form-input>
          <span v-if="$v.user.name.$error" class="errorMsg"
            >Please Enter Your Name</span
          >
        </b-form-group>
      </b-col>

      <b-col cols="4">
        <b-form-group id="address" label="Address" label-for="Address">
          <b-form-input
            id="address"
            type="text"
            v-model="user.address.street"
            placeholder="Reter 43"
          >
          </b-form-input>
          <span v-if="$v.user.address.street.$error" class="errorMsg"
            >Please Enter Your Address</span
          >
        </b-form-group>
      </b-col>
      <b-col cols="2">
        <b-form-checkbox
          id="checkbox-1"
          name="checkbox-1"
          value="accepted"
          unchecked-value="not_accepted"
          @input="showCoordinate"
        >
          Use Google Location
        </b-form-checkbox>
      </b-col>
    </b-row>

    <b-row>
      <b-col cols="6">
        <b-form-group id="username" label="Username" label-for="username">
          <b-form-input
            id="username"
            type="text"
            placeholder="Username"
            v-model="user.username"
          >
          </b-form-input>
          <span v-if="$v.user.username.$error" class="errorMsg"
            >Please Enter Your Username</span
          >
        </b-form-group>
      </b-col>

      <b-col cols="6">
        <b-form-group id="city" label="City" label-for="City">
          <b-form-input
            id="city"
            type="text"
            placeholder="Tirane"
            v-model="user.address.city"
          >
          </b-form-input>
          <span v-if="$v.user.address.city.$error" class="errorMsg"
            >Please Enter Your City</span
          >
        </b-form-group>
      </b-col>
    </b-row>

    <b-row class="mt-3">
      <b-col cols="6">
        <b-form-group id="email" label="E-Mail" label-for="email">
          <b-form-input
            id="email"
            type="email"
            placeholder="name.lastanme@.com"
            v-model="user.email"
          >
          </b-form-input>
          <span v-if="$v.user.email.$error" class="errorMsg"
            >Please Enter Your Email
          </span>
        </b-form-group>
      </b-col>

      <b-col cols="6">
        <b-form-group id="zip_code" label="Zip Code" label-for="Zip Code">
          <b-form-input
            id="zip_code"
            type="text"
            placeholder="Zip Code"
            v-model="user.address.zipcode"
          >
          </b-form-input>
          <span v-if="$v.user.address.zipcode.$error" class="errorMsg"
            >Please Enter Your Zip Code</span
          >
        </b-form-group>
      </b-col>
    </b-row>

    <b-row>
      <b-col cols="6">
        <b-form-group id="phone_nr" label="Phone Nr" label-for="phone_number">
          <b-form-input
            id="phone_nr"
            placeholder="+3556879908"
            v-model="user.phone"
          >
          </b-form-input>
          <span v-if="$v.user.phone.$error" class="errorMsg"
            >Please Enter Your Phone Number
            <span v-if="!$v.user.phone.minLength" class="errorMsg"
              >/ Please Enter Minimum 10 character</span
            >
            <span v-if="!$v.user.phone.maxLength" class="errorMsg"
              >/ Please Enter Maximum 12 character</span
            >
          </span>
        </b-form-group>
      </b-col>

      <b-col cols="3" v-show="!coordinate">
        <b-form-group id="latitude " label="latitude " label-for="latitude ">
          <b-form-input
            id="latitude "
            placeholder="Latitude "
            v-model="user.address.geo.lat"
          >
          </b-form-input>
          <span v-if="$v.user.address.geo.lat.$error" class="errorMsg"
            >Please Enter Your Latitude</span
          >
        </b-form-group>
      </b-col>

      <b-col cols="3" v-show="!coordinate">
        <b-form-group id="longitude " label="longitude " label-for="longitude ">
          <b-form-input
            id="latitude "
            placeholder="Longitude "
            v-model="user.address.geo.lng"
          >
          </b-form-input>
          <span v-if="$v.user.address.geo.lng.$error" class="errorMsg"
            >Please Enter Your Longitude</span
          >
        </b-form-group>
      </b-col>
    </b-row>

    <div class="btn-edi mt-4 mb-4" style="float: right">
      <b-button variant="primary" class="px-5" @click="updateuser"
        >Update user</b-button
      >
    </div>
  </b-form>
</template>

<script>
import axios from "axios";
import {
  required,
  minLength,
  maxLength,
  email,
} from "vuelidate/lib/validators";

export default {
  name: "CreateuserModal",
  props: {
    id: Number,
  },
  data() {
    return {
      coordinate: true,
      user: {
        name: "",
        address: "",
        username: "",
        email: "",
        phone: "",
        address: {
          street: "",
          city: "",
          zipcode: "",
          geo: {
            lat: "",
            lng: "",
          },
        },
      },
    };
  },
  validations: {
    user: {
      name: { required },
      username: { required },
      address: {
        street: { required },
        city: { required },
        zipcode: { required },
        geo: {
          lat: { required },
          lng: { required },
        },
      },
      email: { required, email },
      phone: { required, minLength: minLength(10), maxLength: maxLength(12) },
    },
  },
  mounted() {
    this.getUserID();
  },
  methods: {
    triggerClose() {
      this.$emit("closeEditModal");
    },

    getUserID() {
      axios
        .get(`https://jsonplaceholder.typicode.com/users/${this.id}`)
        .then((response) => {
          this.user = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    updateuser() {
      this.$v.$touch();
      if (this.$v.$invalid) {
        return;
      }

      axios
        .put(`https://jsonplaceholder.typicode.com/users/${this.id}`, this.user)
        .then((response) => {
          console.log(response.data);
          this.$emit("closeEditModal");
          this.$emit("reloadDataTable");
          this.$emit("showSuccessAlert");
        })
        .catch((error) => {
          console.log(error);
        });
    },

    showCoordinate() {
      if (this.coordinate) this.coordinate = false;
      else this.coordinate = true;
    },
  },
};
</script>