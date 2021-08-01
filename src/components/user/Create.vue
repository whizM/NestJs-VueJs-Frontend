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
          <div id="create-post-title" class="col-md-2">
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
          <div class="col-md-2 d-flex justify-content-end">
            <button class="btn btn-success" type="button" v-on:click="getCep">Buscar cep</button>
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
        
        
        
        

        <div class="form-group col-md-12">
          <label for="title"> GitHub </label>
          <input
            type="text"
            id="github"
            v-model="github"
            name="title"
            class="form-control"
            placeholder="usuario do github"
          />
          
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
        girhub: "",
        address: {},
      };
    },
    methods: {
      
      getCep() {
        axios.get(`https://viacep.com.br/ws/${parseInt(this.cep)}/json/`).then((data) => {
          this.address = data.data;
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

</style>
