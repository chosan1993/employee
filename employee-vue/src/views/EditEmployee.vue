<template>
  <div class="edit container mt-12">
    <h1 class="mb-4">Edit Employee</h1>
    <form action="#" method="POST" @submit.prevent="editEmployee">
      <div class="form-group">
        <label class="font-bold mb-2" for="name">Name</label>
        <input type="text" name="name" id="name" v-model="name" />
      </div>
      <div class="form-group">
        <label class="font-bold mb-2" for="email">Email</label>
        <input type="text" name="email" id="email" v-model="email" />
      </div>
      <div class="form-group">
        <label class="font-bold mb-2" for="position">Position</label>
        <input type="text" name="position" id="position" v-model="position" />
      </div>
      <div class="form-group">
        <button type="submit">Update Employee</button>
      </div>
    </form>
  </div>
</template>

<script>
import updateEmployee from "@/graphql/mutations/updateEmployee.gql";
import employee from "@/graphql/queries/Employee.gql";

export default {
  data() {
    return {
      name: "",
      email: "",
      position: "",
    };
  },
  apollo: {
    employees: {
      query: employees,
      variables() {
        if (this.$route && this.$route.params) {
          return {
            id: this.$route.params.id,
          };
        }
      },

      result({ data: { employees } }) {
        this.name = employees.name;
        this.email = employees.email;
        this.position = employees.position;
      },
    },
  },
  methods: {
    editEmployee() {
      this.$apollo
        .mutate({
          mutation: updateEmployee,
          variables: {
            id: this.$route.params.id,
            name: this.name,
            email: this.email,
            position: this.position,
          },
        })
        .then((data) => {
          console.log(data);
          this.$router.push(`/employees/${this.$route.params.id}`);
        })
        .catch((error) => {
          console.error(error);
        });
    },
  },
};
</script>

<style scoped>
.form-group {
  margin-bottom: 32px;
}
input[type="text"],
textarea {
  padding: 10px 14px;
  border: 1px solid lightgray;
  border-radius: 5px;
}

label {
  display: block;
}

button {
  padding: 16px;
  background: #027bff;
  color: white;
  border-radius: 5px;
  font-size: 16px;
}
</style>
