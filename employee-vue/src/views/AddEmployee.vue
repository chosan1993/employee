<template>
  <div class="create container mt-12">
    <h1 class="mb-4">Create Employee</h1>
    <form action="#" method="POST" @submit.prevent="addEmployee">
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
        <button type="submit">Add Employee</button>
      </div>
    </form>
  </div>
</template>

<script>
import addEmployee from "@/graphql/mutations/AddEmployee.gql";

export default {
  data() {
    return {
      name: "",
      email: "",
      position: "",
    };
  },
  methods: {
    addEmployee() {
      this.$apollo
        .mutate({
          mutation: addEmployee,
          variables: {
            name: this.name,
            email: this.email,
            position: this.position,
          },
        })
        .then((data) => {
          console.log(data);
          this.$router.push("/");
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
