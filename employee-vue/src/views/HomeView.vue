<template>
  <div class="container">
    <ApolloQuery :query="require('@/graphql/queries/Employees.gql')">
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <div v-if="isLoading">Loading...</div>
        <table class="table table-striped table-bordered">
          <thead>
            <tr>
              <th>ID</th>
              <th>Name</th>
              <th>Email</th>
              <th>Position</th>
              <th>Action</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="employee in employees" :key="employee.id">
              <td>{{ employee.id }}</td>
              <td>{{ employee.name }}</td>
              <td>{{ employee.email }}</td>
              <td>{{ employee.position }}</td>
              <td>
                <router-link
                  :to="`/employees/${data.employee.id}/edit`"
                  href="#"
                  >Edit</router-link
                >
                <a href="#" @click.prevent="deleteEmployee">Delete</a>
              </td>
            </tr>
          </tbody>
        </table>
      </template>
    </ApolloQuery>
  </div>
</template>

<script>
import deleteEmployee from "@/graphql/mutations/DeleteEmployee.gql";

export default {
  methods: {
    deleteEmployee() {
      this.$apollo
        .mutate({
          mutation: deleteEmployee,
          variables: {
            id: this.$route.params.id,
          },
        })
        .then((data) => {
          console.log(data);
          this.$router.push("/");
        });
    },
  },
};
</script>
