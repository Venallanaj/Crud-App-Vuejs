<template>
  <div>
    <b-row>
      <b-alert v-model="showSuccessAlert" variant="success" dismissible>
        {{ alertMessage }}
      </b-alert>
    </b-row>

    <b-row>
      <b-card>
        <b-row align-h="between">
          <b-col cols="6"></b-col>
          <b-col cols="6">
            <b-row>
              <b-col>
                <b-button
                  style="float: right"
                  variant="primary"
                  id="show-btn"
                  @click="showCreateModal"
                >
                  <b-icon-plus class="text-white"></b-icon-plus>
                  <span class="h6 text-white">Add New User</span>
                </b-button>
              </b-col>
            </b-row>
          </b-col>
        </b-row>
        <b-row class="mt-3">
          <b-table :items="items" :fields="fields" class="text-left">
            <template #cell(contact_name)="data">
              {{
                `${data.item.contact_firstname} ${data.item.contact_lastname}`
              }}
            </template>
            <template #cell(actions)="data">
              <b-row>
                <b-col cols="7">
                  <b-icon-pencil-square
                    class="action-item"
                    variant="muted"
                    @click="getRowData(data.item.id)"
                  ></b-icon-pencil-square>
                </b-col>
                <b-col cols="1">
                  <b-icon-trash-fill
                    class="action-item"
                    variant="muted"
                    @click="showDeleteModal(data.item.id)"
                  ></b-icon-trash-fill>
                </b-col>
              </b-row>
            </template>
          </b-table>
        </b-row>
      </b-card>
    </b-row>

    <!-- Modal for adding new user -->
    <b-modal
      ref="create-user-modal"
      size="xl"
      hide-footer
      title="New User Info"
    >
      <create-user-form
        @closeCreateModal="closeCreateModal"
        @reloadDataTable="getUserData"
        @showSuccessAlert="showAlertCreate"
      ></create-user-form>
    </b-modal>

    <!-- Modal for updating user -->
    <b-modal ref="edit-user-modal" size="xl" hide-footer title="Edit User">
      <edit-user-form
        @closeEditModal="closeEditModal"
        @reloadDataTable="getUserData"
        @showSuccessAlert="showAlertUpdate"
        :id="id"
      ></edit-user-form>
    </b-modal>

    <!-- Delete user Modal -->
    <b-modal
      ref="delete-user-modal"
      size="md"
      hide-footer
      title="Confirm Deletion"
    >
      <delete-user-modal
        @closeDeleteModal="closeDeleteModal"
        @reloadDataTable="getUserData"
        @showDeleteAlert="showDeleteSuccessModal"
        :id="id"
      ></delete-user-modal>
    </b-modal>
  </div>
</template>

<script>
import axios from "axios";
import CreateUserForm from "@/components/CreateUserForm.vue";
import EditUserForm from "@/components/EditUserForm.vue";
import DeleteUserModal from "@/components/DeleteUserModal.vue";

export default {
  components: {
    CreateUserForm,
    EditUserForm,
    DeleteUserModal,
  },
  data() {
    return {
      fields: [
        /**
         *
         */

        {
          key: "id",
          label: "ID",
          sortable: false,
        },

        /**
         *
         */

        {
          key: "name",
          label: "Contact Name",
          sortable: false,
        },

        /**
         *
         */

        {
          key: "email",
          label: "Email",
          sortable: false,
        },

        /**
         *
         */

        {
          key: "phone",
          label: "Phone",
          sortable: false,
        },

        /**
         *
         */

        "actions",
      ],
      items: [],
      id: 0,
      showSuccessAlert: false,
      alertMessage: "",
    };
  },

  mounted() {
    this.getUserData();
  },

  methods: {
    showCreateModal() {
      this.$refs["create-user-modal"].show();
    },

    closeCreateModal() {
      this.$refs["create-user-modal"].hide();
    },

    getUserData() {
      axios
        .get("https://jsonplaceholder.typicode.com/users/")
        .then((response) => {
          this.items = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },

    getRowData(id) {
      this.$refs["edit-user-modal"].show();
      this.id = id;
    },

    closeEditModal() {
      this.$refs["edit-user-modal"].hide();
    },

    showAlertCreate() {
      this.showSuccessAlert = true;
      this.alertMessage = "user was created successfully!";
    },
    showAlertUpdate() {
      this.showSuccessAlert = true;
      this.alertMessage = "User was updated successfully";
    },

    showDeleteModal(id) {
      this.$refs["delete-user-modal"].show();
      this.id = id;
    },

    closeDeleteModal() {
      this.$refs["delete-user-modal"].hide();
    },

    showDeleteSuccessModal() {
      this.showSuccessAlert = true;
      this.alertMessage = "User was deleted successfully!";
    },
  },
};
</script>


