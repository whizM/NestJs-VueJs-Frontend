<template>
  <div>
    <div class="col-md-12 form-wrapper">
      <h2>Cadastrar</h2>
      <form id="create-post-form" @submit.prevent="createCustomer">
        <div class="form-group col-md-12">
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
        <div class="form-group col-md-12">
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
        <div class="form-group col-md-12">
          <label for="title"> Endereço </label>
          <input
            type="text"
            id="address"
            v-model="address"
            name="title"
            class="form-control"
            placeholder="Buscar Cep"
          />
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
          <button class="btn btn-success" type="submit">Criar Usuário</button>
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
      first_name: "",
      last_name: "",
      email: "",
      phone: "",
      address: "",
      description: "",
    };
  },
  methods: {
    createCustomer() {
      let customerData = {
        name: this.name,
        age: this.age,
        address: this.address,
        github: this.github,
      };
      this.__submitToServer(customerData);
    },
    __submitToServer(data) {
      axios.post(`${server.baseURL}/user/create`, data).then((data) => {
        router.push({ name: "home" });
      });
    },
  },
};
</script>
