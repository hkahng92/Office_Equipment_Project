<template>
    <div class="container">
      <div class="content-heading text-center">
        <h1 v-if="!id">Add User</h1>
        <h1 v-if="id">Update User</h1>
      </div>
      <form class="jumbotron jumbotron-fluid">
          <div v-if="errors.length">
          <div class="alert alert-warning" v-bind:key="index" v-for="(error, index) in errors">{{error}}</div>
        </div>
        <div class="form-group">
          <button v-on:click="redirect" type="button" class="close text-danger" aria-label="Close">
            <span aria-hidden="true">
              <i class="far fa-times-circle"></i>
            </span>
          </button>
        </div>
        <div class="form-group">
          <label for="userName">Name</label>
          <input type="text" class="form-control" id="userName" v-model="user.userName" />
        </div>

        <div class="form-group">
          <label for="inputAddress">Address</label>
          <input
            type="text"
            class="form-control"
            id="inputAddress"
            v-model="user.streetAddress"
            placeholder="1234 Main St"
          />
        </div>
        <div class="form-group">
          <label for="inputAddress2">Address 2</label>
          <input
            type="text"
            class="form-control"
            id="inputAddress2"
            v-model="user.address2"
            placeholder="Apartment, studio, or floor"
          />
        </div>
        <div class="form-row">
          <div class="form-group col-md-6">
            <label for="inputCity">City</label>
            <input type="text" class="form-control" id="inputCity" v-model="user.city" />
          </div>
          <div class="form-group col-md-4">
            <label for="inputState">State</label>
            <input id="inputState" class="form-control" />
          </div>
          <div class="form-group col-md-2">
            <label for="inputZip">Zip</label>
            <input type="text" class="form-control" id="inputZip" v-model="user.zip" />
          </div>
        </div>
        <div class="form-group text-center">
          <button @click="validateAndSubmit" class="btn btn-lg btn-primary">Save</button>
          <button @click="cancelForm" class="btn btn-lg btn-danger ml-2">Cancel</button>
        </div>
      </form>
    </div>
</template>

<script>
import OfficeDataService from "../../service/common/UserDataService";
export default {
  name: "user",
  data() {
    return {
      user: {
        userName: '',
        streetAddress: '',
        city: '',
        zip: '',
        active: true
      },
      id: this.$route.query.id,
      errors: []
    };
  },
  created() {
    console.log("Form Created");
    console.log('params: ' + this.$router.query.id);
    if(this.id){
      UserDataService.getUserById(this.id).then( result => {
        this.user = result;
      });
    }
  },
  computed: {
    // id() {
    //   return this.$route.query.id;
    // }
  },
  methods: {

      cancelForm: function(event){
        event.preventDefault();
        this.$router.push("/userList");
      },

      redirect: function (event) {
       this.$router.push("/userList");
      },
    //this code checks the validity of the fields

    validateAndSubmit(e) {
      e.preventDefault();
      this.errors = [];
      if (!this.user.userName) {
        this.errors.push("Enter valid values");
      }
      if (!this.user.streetAddress) {
        this.errors.push("Enter valid values");
      }
      if (!this.user.city) {
        this.errors.push("Enter valid values");
      }
      if (!this.user.zip) {
        this.errors.push("Enter valid values");
      }

      //When the user input is valid, if there is no id in the path
      //then the user is saved to the database and the app is routed to officeList
      if (this.errors.length === 0) {
        if (!this.id) {
        //   userDataService.createUser(this.user).then(() => {
            // this.$router.push("/officeList");
        //   });
        }

        //When the user input is valid, if there is id in the path
        //then the office is updated in the database and the app is routed to officeList
        else {
          OfficeDataService.updateOffice(this.id, this.office).then(() => {
            this.$router.push("/userList");
          });
        }
      }
    }
  }
};
</script>
