<template>
  <div>
    <h4 class="text-center mt-20">
      <small>
        <button class="btn btn-success" v-on:click="navigate()">
          todos
        </button>
      </small>
    </h4>
    <div class="col-md-12 form-wrapper">
      <h2>Editar usuario</h2>
      <form id="create-post-form" @submit.prevent="editUser">
        <div class="form-group col-md-12">
          <label for="title"> Name </label>
          <input
            type="text"
            id="name"
            v-model="users.name"
            name="title"
            class="form-control"
            placeholder="Nome"
          />
        </div>
        <div class="form-group col-md-12">
          <label for="title"> Idade </label>
          <input
            type="text"
            id="age"
            v-model="users.age"
            name="title"
            class="form-control"
            placeholder="Idade"
          />
        </div>
        <div class="form-group col-md-12">
          <label for="title"> Address </label>
          <input
            type="text"
            id="address"
            v-model="users.address"
            name="title"
            class="form-control"
            placeholder="Endereço"
          />
        </div>
        <div class="form-group col-md-12">
          <label for="title"> Github </label>
          <input
            type="text"
            id="github"
            v-model="users.github"
            name="title"
            class="form-control"
            placeholder="Githuib"
          />
        </div>

        <div class="form-group col-md-4 pull-right">
          <button class="btn btn-success" type="submit">Editar usuario</button>
        </div>
      </form>
    </div>
  </div>
</template>
<script>
import { server } from "../../helper";
import axios from "axios";
import router from "../../router";
export default {
  data() {
    return {
      id: 0,
      users: {},
    };
  },
  created() {
    this.id = this.$route.params.id;
    this.getUser();
  },
  methods: {
    editUser() {
      let body = {
        name: this.users.name,
        age: this.users.age,
        address: this.users.address,
        github: this.users.github,
      };
      console.log(body)
      axios
        .put(`http://localhost:3000/users/${this.id}`, body)
        .then((data) => {
          router.push({ name: "home" });
        });
    },
    getUser() {
      axios
        .get(`http://localhost:3000/users/${this.id}`)
        .then((data) => (this.users = data.data));
    },
    navigate() {
      router.go(-1);
    },
  },
};
</script>
