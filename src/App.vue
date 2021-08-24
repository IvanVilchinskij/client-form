<template>
  <div id="app">
    <div class="content">
      <div class="client-form">
        <h1 class="client-form__title">Создание клиента</h1>
        <Form :defValidForm="defValidForm" />
      </div>
      <div class="bg">
        <img src="./assets/img/bg.png" alt="main-bg" />
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
@import "./assets/styles/modal.scss";
</style>
