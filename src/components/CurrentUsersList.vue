<template>
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
</template>

<script>

export default {
  name: 'CurrentUsersList',
  methods: {
    deleteUser: function (user_id) {
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
            // this.users.forEach((user, index) => {
            //   if (user.id === user_id) {
            //     this.users.splice(index, 1);
            //   }
            // });
          });
    }
  },

  props: {
    users: String,
  }
}
</script>

<style>

.table-text-left {
  text-align: left;
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

.active-button {
  background-color: #32CD32;
}

.inactive-button {
  background-color: #FF6347;
}
</style>
