<template>
  <div class="container-fluid">
    <div class="text-center">
      <h1>Listagem de pessoas cadastradas</h1>
      <div v-if="users.length === 0"></div>
    </div>

    <div class="">
      <table class="table table-bordered">
        <thead class="thead-dark">
          <tr>
            <th scope="col">Nome</th>
            <th scope="col">Idade</th>
            <th scope="col">Endereço</th>
            <th scope="col">GitHub</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="user in users" :key="user.id">
            <td>{{ user.name }}</td>
            <td>{{ user.age }}</td>
            <td>{{ user.address }}</td>
            <td>{{ user.github }}</td>
            <td>
              <div class="d-flex justify-content-between align-items-center">
                <div class="btn-group" style="margin-bottom: 20px;">
                  <router-link
                    :to="{ name: 'Edit', params: { id: user.id } }"
                    class="btn btn-sm btn-outline-secondary"
                    >Editar
                  </router-link>
                  <button
                    class="btn btn-sm btn-outline-secondary"
                    v-on:click="deleteUser(user.id)"
                  >
                    Deletar
                  </button>
                </div>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import { server } from "../helper";
import axios from "axios";
export default {
  data() {
    return {
      users: [],
    };
  },

  created() {
    this.getAllUsers();
  },

  methods: {
    getAllUsers() {
      axios
        .get(`http://localhost:3000/users`)
        .then((data) => {
          this.users = data.data
        });
    },
    deleteUser(id) {
      axios
        .delete(`http://localhost:3000/users/${id}`)
        .then((data) => {
          console.log(data)
          window.location.reload();
        });
    },
  },
};
</script>
