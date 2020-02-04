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
export default {
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
  }
};
</script>