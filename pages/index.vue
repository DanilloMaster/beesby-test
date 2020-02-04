<template>
  <v-form class="login-form">
    <v-container fluid>
      <v-select :items="items" v-model="model"></v-select>
      <v-text-field label="Candidato" v-model="candidate"></v-text-field>
      <div class="my-2">
        <v-btn @click="test">INICIAR</v-btn>
      </div>
    </v-container>
  </v-form>
</template>

<script>
import { validationMixin } from "vuelidate";
import { required, email } from "vuelidate/lib/validators";
export default {
  mixins: [validationMixin],

  validations: {
    email: { required, email },
    pass: { required }
  },

  data() {
    return {
      email: "",
      pass: "",
      showPass: false,
      items: [
        { text: "Jr", values: "Jr" },
        { text: "Estagiario", value: "Estagiario" }
      ],
      model: "Jr",
      candidate: ""
    };
  },

  methods: {
    test() {
      this.$router.push({ path: "/test", query: { nome: this.candidate } });
    }
  },

  computed: {
    passErrors() {
      const errors = [];
      if (!this.$v.pass.$dirty) return errors;
      !this.$v.pass.required && errors.push("Senha é obrigatória.");
      return errors;
    },
    emailErrors() {
      const errors = [];
      if (!this.$v.email.$dirty) return errors;
      !this.$v.email.email && errors.push("Insira um email válido");
      !this.$v.email.required && errors.push("Email é obrigatório");
      return errors;
    }
  }
};
</script>