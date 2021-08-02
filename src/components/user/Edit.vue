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
      <h2>Editar Usuário</h2>
      <form id="create-post-form" @submit.prevent="editUser">
        <div class="form-group d-flex">
          <div id="create-post-title" class="col-md-8">
            <label for="title"> Nome </label>
            <input
              type="text"
              id="name"
              v-model="users.name"
              name="title"
              class="form-control"
            />
          </div>
            <div class="col-md-4">
              <label for="title"> Idade </label>
              <input
                type="text"
                id="age"
                v-model="users.age"
                name="title"
                class="form-control"
              />
            </div>
        </div>

        <div class="form-group d-flex">
          <div id="create-post-title" class="col-md-8">
            <label for="title"> Cep </label>
            <input
              type="text"
              id="cep"
              v-model="users.cep"
              name="title"
              class="form-control"
            />
          </div>
          
          <div id="create-post-title" class="col-md-4">
            <label for="title"> UF </label>
            <input
              type="text"
              id="state"
              v-model="users.state"
              name="title"
              class="form-control"
            />
          </div>
        </div>
        
        <div class="form-group d-flex">
          <div id="create-post-title" class="col-md-4">
            <label for="title"> Rua </label>
            <input
              type="text"
              id="street"
              v-model="users.street"
              name="title"
              class="form-control"
            />
          </div>
          <div id="create-post-title" class="col-md-4">
            <label for="title"> Bairro </label>
            <input
              type="text"
              id="neighbourhood"
              v-model="users.neighbourhood"
              name="title"
              class="form-control"
            />
          </div>
          <div id="create-post-title" class="col-md-4">
            <label for="title"> Cidade </label>
            <input
              type="text"
              id="city"
              v-model="users.city"
              name="title"
              class="form-control"
            />
          </div>
        </div>
        
        <div class="form-group d-flex">
          <div id="create-post-title" class="col-md-8">
            <label for="title"> GitHub </label>
            <input
              type="text"
              id="github"
              v-model="users.github"
              name="title"
              class="form-control"
            />
          </div>

          <div id="create-post-title" class="col-md-4">
            <label for="title"> Id </label>
            <input
              type="text"
              id="githubId"
              v-model="users.githubId"
              name="title"
              class="form-control"
            />
          </div>
          
        </div>
        
        <div class="form-group d-flex">
          <div id="create-post-title" class="col-md-2">
            <img class="card-image-top" :src="users.avatar_url" />
          </div>
          <div>
            <div class="col-md-12 d-flex flex-column align-items-start">
              <label class="m-0" for="title" id="nameUser"> Nome:&nbsp;<strong>{{  users.nameUser  }}</strong></label>
              <label class="m-0" for="title" id="bio"> Descrição:&nbsp;<strong>{{  users.bio  }}</strong></label>
              <label class="m-0" for="title" id="desc"> Descrição:</label>
              <ul id="create-list-repo">
                  <li v-for="repo in repos" :key="repo.id">
                    <span class="m-0" id="github">Name:&nbsp;<strong>{{  repo.name  }}</strong></span>
                    <span class="m-0" id="github">Descrição:&nbsp;<strong>{{  repo.full_name  }}</strong></span>
                    <span class="m-0" id="github">Linguagem:&nbsp;<strong>{{  repo.language  }}</strong></span>
                  </li>
                </ul>
            </div>  
          </div>
        </div>

        <div class="form-group col-md-4 pull-right">
          <button class="btn btn-success" type="submit">Editar</button>
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
      repos: {},
    };
  },
  created() {
    this.id = this.$route.params.id;
    this.getUser();
  },
  methods: {
    getUser() {
      axios
        .get(`http://localhost:3000/users/${this.id}`)
        .then((response) => {
          this.users = response.data
          axios.get(`https://api.github.com/users/${this.users.github}/repos`).then((response, i) => {
            const arrRepos = []
            response.data.forEach(item => {
              var body = {
                  name: item.name,
                  full_name: item.full_name,
                  id: item.id,
                  language: item.language == null ? "" : item.language,
              }
              arrRepos.push(body)
            })
          this.repos = arrRepos
          })
        });
    },
    
    navigate() {
      router.go(-1);
    },

    editUser() {
      let body = {
        id: this.users.id,
        name: this.users.name,
        age: this.users.age,
        cep: this.users.cep,
        street: this.users.street,
        neighbourhood: this.users.neighbourhood,
        city: this.users.city,
        state: this.users.state,
        github: this.users.github,
        githubId: this.users.githubId,
        nameUser: this.users.nameUser,
        avatar_url: this.users.avatar_url,
        bio: this.users.bio
      };
      axios.put(`http://localhost:3000/users/${this.users.id}`, body)
        .then((response) => {
          router.push({ name: "home" });
        });
    },

  },
};
</script>
