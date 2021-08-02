<template>
  <div>
    <div class="col-md-12 form-wrapper">
      <h2>Cadastrar Usuário</h2>
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
            />
          </div>
        </div>
        
        <div class="form-group d-flex">
          <div id="create-post-title" class="col-md-8">
            <label for="title"> GitHub </label>
            <input
              type="text"
              id="github"
              v-model="github"
              name="title"
              class="form-control"
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
            />
          </div>
          
        </div>
        <div v-if=" avatar_url ">
          <div class="form-group d-flex">
            <div id="create-post-title" class="col-md-2">
              <img class="card-image-top" :src="avatar_url" />
            </div>
            <div>
              <div class="col-md-12 d-flex flex-column align-items-start">
                <label class="m-0" for="title" id="github"> Nome:&nbsp;<strong>{{  nameUser  }}</strong></label>
                <label class="m-0" for="title" id="github"> Descrição:&nbsp;<strong>{{  bio  }}</strong></label>
                <label class="m-0" for="title" id="github"> Descrição:</label>

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
        nameUser: "",
        avatar_url: "",
        bio: "",
        repos: {},
      };
    },
    methods: {
      
      getCep() {
        axios.get(`https://viacep.com.br/ws/${parseInt(this.cep)}/json/`).then((response) => {
          this.address = response.data;
        });
      },

      getGithub() {
        axios.get(`https://api.github.com/users/${this.github}`).then((response) => {
          console.log(response.data)
          this.githubId = response.data.id
          this.nameUser = response.data.name
          this.avatar_url = response.data.avatar_url
          this.bio = response.data.bio
          const repos = response.data.repos_url 
          
          axios.get(`https://api.github.com/users/${this.github}/repos`).then((response, i) => {
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
          githubId: this.githubId,
          nameUser: this.nameUser,
          avatar_url: this.avatar_url,
          bio: this.bio
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

  #create-post-form label,span {
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

  #create-post-form ul {
    margin: 0;
    padding: 10px;
  }

  #create-post-form li {
    width: 490px;
    border-radius: 5px;
    padding: 5px 10px;
    margin: 5px;
    background: #c3c3c3;
    list-style-type: none;
    cursor: pointer;
    transition: width 0.5s;
  }

  #create-post-form li:hover {
    width: 500px;
  }

  #create-post-form input {
    color: #808080;
  }

</style>
