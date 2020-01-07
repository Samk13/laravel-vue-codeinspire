<template>
  <div class="container">
    <div class="row mt-5">
      <div class="col-md-12">
        <div class="card">
          <div class="card-header">
            <h3 class="card-title">Users table</h3>

            <div class="card-tools">
              <button class="btn btn-success" data-toggle="modal" data-target="#addNewUser">
                <i class="fas fa-user-plus"></i>
              </button>
            </div>
          </div>
          <!-- /.card-header -->
          <div class="card-body table-responsive p-0">
            <table class="table table-hover">
              <thead>
                <tr>
                  <th>ID</th>
                  <th>Name</th>
                  <th>Email</th>
                  <th>Type</th>
                  <th>Registered at</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="user in users" :key="user.id">
                  <td>{{user.id}}</td>
                  <td>{{ user.name}}</td>
                  <td>{{user.email}}</td>
                  <td>{{user.type | toUpper}}</td>
                  <td>{{user.created_at|datePretty}}</td>
               
                  <td>
                    <a href="#">
                      <i class="fa fa-users-cog green"></i>
                    </a>
                    /
                    <a href="#">
                      <i class="fa fa-user-slash red"></i>
                    </a>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
          <!-- /.card-body -->
        </div>
        <!-- /.card -->
      </div>
    </div>

    <!-- Modal -->

    <div
      class="modal fade"
      id="addNewUser"
      aria-labelledby="addNewLabel"
      tabindex="-1"
      role="dialog"
    >
      <div class="modal-dialog modal-dialog-centered" role="document">
        <div class="modal-content">
          <!-- modal Header -->
          <div class="modal-header">
            <h5 class="modal-title" id="addNewLabel">Add new User</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <!-- /modal header -->
          <!-- Body  -->
          <form @submit.prevent="createUser">
             <div class="modal-body">
            <p>🙋‍♀️</p>
          <!-- name  -->
            <div class="form-group">
              <input
                v-model="form.name"
                type="text"
                name="name"
                placeholder="full name"
                class="form-control"
                :class="{ 'is-invalid': form.errors.has('name') }"
              />
              <has-error :form="form" field="name"></has-error>
            </div>
            <!-- /name -->
            <!-- email -->
            <div class="form-group">
              <input
                v-model="form.email"
                type="text"
                name="email"
                placeholder="Email"
                class="form-control"
                :class="{ 'is-invalid': form.errors.has('email') }"
              />
              <has-error :form="form" field="email"></has-error>
            </div>
            <!-- /email -->
            <!-- bio -->
            <div class="form-group">
              <textarea
                v-model="form.bio"
                id="bio"
                name="bio"
                placeholder="describe yourself 'optional'"
                class="form-control"
                :class="{ 'is-invalid': form.errors.has('bio') }"
              ></textarea>
              <has-error :form="form" field="bio"></has-error>
            </div>
            <!-- /bio -->
            <!-- user type -->
             <div class="form-group">
              <select
                v-model="form.type"
                type="text"
                name="type"
                id="type"
                placeholder="User type -> admin or user"
                class="form-control"
                :class="{ 'is-invalid': form.errors.has('type') }"
              >
              <option value="">Select User Role</option>
              <option value="admin">Admin</option>
              <option value="user">Standard User</option>
              <option value="author">Author</option>
              </select>
              <has-error :form="form" field="type"></has-error>
            </div>
            <!-- /user type -->
            <!-- password -->
             <div class="form-group">
              <input
                v-model="form.password"
                type="password"
                name="password"
                placeholder="password"
                class="form-control"
                :class="{ 'is-invalid': form.errors.has('password') }"
              />
              <has-error :form="form" field="password"></has-error>
            </div>
            <!-- /password -->
          </div>
          <!-- /body -->

          
          <!-- footer -->
          <div class="modal-footer">
            <button type="submit" class="btn btn-primary">
              <i class="fa fa-user-plus"></i>
            </button>
            <button type="button" class="btn btn-secondary" data-dismiss="modal">
              <i class="fa fa-skull"></i>
            </button>
          </div>
          </form>
          <!-- footer -->
        </div>
        
      </div>
    </div>

    <!--/ Modal -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      users : {},
      form: new Form({
        name: "",
        email: "",
        password: "",
        type: "",
        bio: "",
        photo: ""
      })
    };
  },
  methods:{
    loadUsers(){
      axios.get("api/user").then(({data}) => (this.users = data.data));
    },
    createUser(){
      this.$Progress.start();
      this.form.post('api/user');

      $('#addNewUser').modal('hide')
      toast.fire({
            icon: 'success',
            title: 'user saved successfully 👏'
          });
      this.$Progress.finish();
    }
  },
  created() {
    this.loadUsers();
  }
};
</script>
