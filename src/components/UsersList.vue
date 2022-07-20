<template>
  <div class="container">
    <div class="row mt-3">
      <div class="col-lg-6">
        <h3 class="heading text-left">Users</h3>
      </div>
      <div class="col-lg-6 add-button-wrapper">
        <button class="add-button btn btn-light" @click="showModal=true">
          <i class="fas fa-user"></i>&nbsp;+&nbsp;Add
        </button>
      </div>
    </div>
    <div class="row">
      <div class="col-lg-12">
        <table class="table">
          <thead>
            <tr class="table-header table-text-left">
              <th>NAME</th>
              <th>EMAIL</th>
              <th>GENDER</th>
              <th>STATUS</th>
              <th></th>
            </tr>
          </thead>
          <tbody>
          <tr class="table-row table-text-left" v-for="user in users" :key="user.id">
            <td>{{user.name}}</td>
            <td>{{user.email}}</td>
            <td>{{user.gender}}</td>
            <td v-if="user.status ==='active'">
              <button type="button" class="active-button status-button btn btn-success" data-dismiss="modal">Active</button>
            </td>
            <td v-else>
              <button type="button" class="inactive-button status-button btn btn-danger" data-dismiss="modal">Inactive</button>
            </td>
            <td @click="deleteUser(user.id)" class="delete">Delete</td>
          </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>

<!--  Add new user - Needs to be added in separate component for readability-->
  <div id="overlay" v-if="showModal">
    <div class="modal add-user" id="myModal" style="display: block;">
      <div class="modal-dialog">
        <div class="modal-content">
          <!-- Modal Header -->
          <div class="modal-header">
            <h4 class="modal-title">Add User</h4>
          </div>
          <!-- Modal body -->
          <div class="modal-body col-md-12">
            <form>
              <div class="form-group row">
                <label class="col-md-4 text-left">Name</label>
                <div class="col-md-12">
                  <input type="text" class="form-control" placeholder="Enter User Name" v-model="newUser.name">
                </div>
              </div>
              <br>
              <div class="form-group row">
                <label for="exampleInputEmail1" class="col-md-4 text-left">Email</label>
                <div class="col-md-12">
                  <input type="email" class="form-control" id="exampleInputEmail1" placeholder="Enter email" v-model="newUser.email">
                </div>
              </div>
              <br>
              <div class="form-group row">
                <label class="col-md-4 text-left">Gender</label>
                <div>
                  <div class="form-check form-check-inline">
                    <input class="form-check-input" type="radio" name="genderRadioOptions" id="inlineRadio1" value="male" v-model="newUser.gender">
                    <label class="form-check-label" for="inlineRadio1">Male</label>
                  </div>
                  <div class="form-check form-check-inline">
                    <input class="form-check-input" type="radio" name="genderRadioOptions" id="inlineRadio2" value="female" v-model="newUser.gender">
                    <label class="form-check-label" for="inlineRadio2">Female</label>
                  </div>
                </div>
              </div>
              <br>
              <div class="form-group row">
                <label class="col-md-4 text-left">Status</label>
                <div>
                  <div class="form-check form-check-inline">
                    <input class="form-check-input" type="radio" name="statusRadioOptions" id="inlineRadio3" value="active" v-model="newUser.status">
                    <label class="form-check-label" for="inlineRadio3">Active</label>
                  </div>
                  <div class="form-check form-check-inline">
                    <input class="form-check-input" type="radio" name="statusRadioOptions" id="inlineRadio4" value="inactive" v-model="newUser.status">
                    <label class="form-check-label" for="inlineRadio4">InActive</label>
                  </div>
                </div>
              </div>
            </form>
          </div>
          <!-- Modal footer -->
          <div class="modal-footer">
            <button type="button" @click="showModal=false; addUser()" class="save-button" data-dismiss="modal">Save</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'UsersList',
  data() {
    return {
      showModal: false,
      users: [],
      newUser: {name:"",email:"",gender:"",status:""}
    }
  },

  mounted: function () {
    this.getAllUsers();
  },

  methods: {
    getAllUsers() {
      // GET request
      const headers = { "Content-Type": "application/json", "Authorisation":"e3d497998ca18d76b283777fb9a5643f73199652d94105e666bc35f5e2c59adb" };
      fetch("https://gorest.co.in/public/v2/users", { headers })
          .then(response => response.json())
          .then(data => {
            this.users = data;
          });
    },

    addUser() {
      // POST request
      const requestOptions = {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'Authorization': 'Bearer e3d497998ca18d76b283777fb9a5643f73199652d94105e666bc35f5e2c59adb',
        },
        body: JSON.stringify(this.newUser)
      };
      fetch('https://gorest.co.in/public/v2/users', requestOptions)
        .then(response => response.json())
          .then((data) => {
            this.users.push(data);
            this.newUser = {name:"",email:"",gender:"",status:""};
          });
    },

    deleteUser(user_id) {
      console.log(user_id);

      const requestOptions = {
        method: 'DELETE',
        headers: {
          'Authorization': 'Bearer e3d497998ca18d76b283777fb9a5643f73199652d94105e666bc35f5e2c59adb',
        }
      };
      fetch('https://gorest.co.in/public/v2/users/' + user_id, requestOptions)
        .then(() => {
          console.log('Delete successful');
          this.users.forEach((user, index) => {
            if (user.id === user_id) {
              this.users.splice(index, 1);
            }
          });
        });
    }
  }
}
</script>

<style>
#overlay {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background: rgba(0,0,0,0.6);
}

.modal-content {
  width: 525px !important;
  height: 550px !important;
}

.modal-header {
  height: auto;
  margin: 0 auto;
  padding: 10px;
  position: relative;
}

.text-left {
  text-align: left;
  font-weight: bold;
  margin-bottom: 10px;
}

.table-text-left {
  text-align: left;
}

.form-check-inline {
  margin-left: -15em;
  margin-bottom: 14px;
}

.save-button {
  display: flex;
  width: 100%;
  margin-left: 21px;
  margin-right: 21px;
  border-radius: 20px;
  background-color: yellow;
  text-align: center;
  border: none;
  align-items: center;
  line-height: 38px;
  font-weight: bold;
  justify-content: center;
}

.table-header {
  height: 32px;
  color: grey;
}

.table-row {
  height: 64px;
}

.status-button {
  border-radius: 17px;
  border: none;
}

.delete {
  color: red;
  cursor: pointer;
}

.add-button {
  border-radius: 20px;
  width: 140px;
  height: 48px;
  background-color: #F0F0F0;
  font-weight: bold;
  float: right;
}

.add-button-wrapper {
  position: relative;
}

.active-button {
  background-color: #32CD32;
}

.inactive-button {
  background-color: #FF6347;
}
</style>
