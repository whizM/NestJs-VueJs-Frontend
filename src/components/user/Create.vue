<template>
  <div>
    <div class="col-md-12 form-wrapper">
      <h2>Cadastrar Pessoa</h2>
      <form id="create-post-form" @submit.prevent="createUser">
        <div class="form-group d-flex">
          <div id="create-post-title" class="col-md-8">
            <label for="title"> Nome </label>
            <input
              type="text"
              id="name"
              v-model="name"
              name="title"
              class="form-control"
              placeholder="Fernando Melo"
            />
          </div>
            <div class="col-md-4">
              <label for="title"> Idade </label>
              <input
                type="text"
                id="age"
                v-model="age"
                name="title"
                class="form-control"
                placeholder="Idade"
              />
            </div>
        </div>

        <div class="form-group d-flex">
          <div id="create-post-title" class="col-md-8">
            <label for="title"> Cep </label>
            <input
              type="text"
              id="cep"
              v-model="cep"
              name="title"
              class="form-control"
              placeholder="00000-000"
            />
          </div>

          <div id="create-post-title" class="col-md-1 d-flex">
            <button class="btn btn-success" type="button" v-on:click="getCep">
              <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-search" viewBox="0 0 16 16">
                <path d="M11.742 10.344a6.5 6.5 0 1 0-1.397 1.398h-.001c.03.04.062.078.098.115l3.85 3.85a1 1 0 0 0 1.415-1.414l-3.85-3.85a1.007 1.007 0 0 0-.115-.1zM12 6.5a5.5 5.5 0 1 1-11 0 5.5 5.5 0 0 1 11 0z"/>
              </svg>
            </button>
          </div>

          <div id="create-post-title" class="col-md-3">
            <label for="title"> UF </label>
            <input
              type="text"
              id="uf"
              v-model="address.uf"
              name="title"
              class="form-control"
              placeholder="SP"
            />
          </div>
          
        </div>
        
        <div class="form-group d-flex">
          <div id="create-post-title" class="col-md-4">
            <label for="title"> Rua </label>
            <input
              type="text"
              id="logradouro"
              v-model="address.logradouro"
              name="title"
              class="form-control"
              placeholder="Logradouro"
            />
          </div>
          <div id="create-post-title" class="col-md-4">
            <label for="title"> Bairro </label>
            <input
              type="text"
              id="bairro"
              v-model="address.bairro"
              name="title"
              class="form-control"
              placeholder="Bairro"
            />
          </div>
          <div id="create-post-title" class="col-md-4">
            <label for="title"> Cidade </label>
            <input
              type="text"
              id="localidade"
              v-model="address.localidade"
              name="title"
              class="form-control"
              placeholder="Cidade"
            />
          </div>
        </div>
        
        <div class="form-group d-flex">
          <div id="create-post-title" class="col-md-8">
            <label for="title"> Username GitHub </label>
            <input
              type="text"
              id="github"
              v-model="github"
              name="title"
              class="form-control"
              placeholder="vue"
            />
          </div>

          <div id="create-post-title" class="col-md-1 d-flex">
            <button class="btn btn-success" type="button" v-on:click="getGithub">
              <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-search" viewBox="0 0 16 16">
                <path d="M11.742 10.344a6.5 6.5 0 1 0-1.397 1.398h-.001c.03.04.062.078.098.115l3.85 3.85a1 1 0 0 0 1.415-1.414l-3.85-3.85a1.007 1.007 0 0 0-.115-.1zM12 6.5a5.5 5.5 0 1 1-11 0 5.5 5.5 0 0 1 11 0z"/>
              </svg>
            </button>
          </div>

          <div id="create-post-title" class="col-md-3">
            <label for="title"> Id </label>
            <input
              type="text"
              id="githubId"
              v-model="githubId"
              name="title"
              class="form-control"
              placeholder="001101011"
            />
          </div>
          
        </div>

        <div class="form-group d-flex">
          <div id="create-post-title" class="col-md-2">
            <img class="card-image-top" :src="avatar_url" />
          </div>
          <div>
            <div id="create-post-title" class="col-md-8 d-flex flex-row align-items-center">
              <label for="title" id="github"> Login: <strong>{{ github }}</strong></label>
              
              <ul v-for="repo in repos" :key="repo.id">
                <label for="title" id="github"> Repositórios: <strong>{{ repo[i].full_name }}</strong></label>
              </ul>
            </div>  
          </div>
          
        </div>

        <div class="form-group col-md-4 pull-right">
          <button class="btn btn-success" type="submit">Cadastrar</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
  import axios from "axios";
  import { server } from "../../helper";
  import router from "../../router";
  export default {
    data() {
      return {
        name: "",
        age: "",
        cep: "",
        github: "",
        address: {},
        githubId: "",
        avatar_url: "",
        login: "",
        repos: [],
      };
    },
    methods: {
      
      getCep() {
        axios.get(`https://viacep.com.br/ws/${parseInt(this.cep)}/json/`).then((data) => {
          this.address = data.data;
        });
      },

      getGithub() {
        axios.get(`https://api.github.com/search/users?q=${this.github}`).then((data) => {
          console.log(data.data)
          this.githubId = data.data.items[0].id;
          this.avatar_url = data.data.items[0].avatar_url;
          this.login = data.data.items[0].login;
          const repos = data.data.items[0].repos_url
          axios.get(`${repos}`).then((data) => {
            console.log(data.data)
            this.repos = data
          })
          
        });
      },

      createUser() {
        let userData = {
          name: this.name,
          age: this.age,
          cep: this.cep,
          street: this.address.logradouro,
          neighbourhood: this.address.bairro,
          city: this.address.localidade,
          state: this.address.uf,
          github: this.github,
        };
        this.__submitToServer(userData);
      },
      __submitToServer(data) {
        axios.post(`http://localhost:3000/users`, data).then((data) => {
          console.log(data.data), router.push({ name: "home" });
        });
      },
    },
  };
</script>
<style>
  #create-post-form {
    background-color: #D3D3D3;
    border-radius: 10px;
    padding: 10px 0;
  }

  #create-post-form label {
    color: #808080;
    display: flex;
  }

  #create-post-form button {
    display: flex;
    justify-content: center;
    align-items: center;
    align-content: center;
    align-self: flex-end;
    height: 40px;
  }

  #create-post-form img {
    width: 100px;
    height: 100px;
    border-radius: 5px;
  }

</style>
