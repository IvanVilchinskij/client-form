<template>
  <div id="app">
    <img src="./assets/img/bg.jpg" alt="bg" class="bg" />
    <div class="container">
      <h1 class="title">Создание клиента</h1>
      <div class="client-form">
        <Form :defValidForm="defValidForm" />
      </div>
    </div>
    <SuccessModal :isValidForm="isValidForm" />
  </div>
</template>

<script>
import Form from "./components/Form";
import SuccessModal from "./components/SuccessModal";

export default {
  name: "App",
  data() {
    return {
      isValidForm: null,
    };
  },
  methods: {
    defValidForm(isValid) {
      this.isValidForm = isValid;
    },
  },
  components: {
    Form,
    SuccessModal,
  },
};
</script>

<style lang="scss">
@import "./assets/styles/reset.scss";
@import "./assets/styles/vars.scss";

@import "./assets/styles/app.scss";
@import "./assets/styles/form.scss";
@import "./assets/styles/errors.scss";
@import "./assets/styles/modal.scss";
</style>
