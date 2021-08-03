<template>
  <div class="container-fluid">
    <div class="text-center">
      <h1>Listagem de Usuários</h1>
      <div v-if="users.length === 0"></div>
    </div>

    <div class="d-flex justify-content-center">
      <table class="table table-bordered w-75 ">
        <thead class="thead-light p-0 m-0">
          <tr>
            <th scope="col">Nome</th>
            <th scope="col">Idade</th>
            <th scope="col">Endereço</th>
            <th scope="col">GitHub</th>
            <th scope="col">Ações</th>
          </tr>
        </thead>
        <tbody class="">
          <tr v-for="user in users" :key="user.id">
            <td class="m-0 p-2">{{ user.name }}</td>
            <td class="m-0 p-2">{{ user.age }}</td>
            <td class="m-0 p-2">{{ user.cep }}</td>
            <td class="m-0 p-2">{{ user.github }}</td>
            <td class="m-0 p-2">
              <div class="d-flex justify-content-center align-items-center">
                <div class="btn-group">
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
