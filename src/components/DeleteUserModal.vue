<template>
  <div>
    <b-row class="mt-2 mb-3">
      <h6 class="text-secondary">
        Are you sure you want to delete this user from tabel?
      </h6>
    </b-row>
    <b-row class="mt-2 mb-3">
      <p class="text-danger">
        This action is not reversible and may result in the loss if important
        data.
      </p>
    </b-row>
    <div class="btn-delete mt-4 mb-4" style="float: right">
      <b-button variant="danger" @click="removeUserFromData"
        >Delete Customer</b-button
      >
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "DeleteUserModal",
  props: {
    customerId: Number,
  },
  methods: {
    triggerClose() {
      this.$emit("closeDeleteModal");
    },
    removeUserFromData() {
      axios
        .delete(`https://jsonplaceholder.typicode.com/users/${this.Id}`)
        .then(() => {
          this.$emit("reloadDataTable");
          this.$emit("showDeleteAlert");
          this.$emit("closeDeleteModal");
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>