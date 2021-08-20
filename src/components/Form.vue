<template>
  <form class="form" @submit.prevent="createdClient($event)">
    <h2 class="form__title">Личные данные:</h2>
    <UserData ref="UserData" :noNumber="noNumber" :onlyNumber="onlyNumber" />
    <h2 class="form__title">Адрес:</h2>
    <AddressData
      ref="AddressData"
      :noNumber="noNumber"
      :onlyNumber="onlyNumber"
    />
    <h2 class="form__title">Паспорт:</h2>
    <PasportData
      ref="PasportData"
      :noNumber="noNumber"
      :onlyNumber="onlyNumber"
    />
    <div class="form__bottom">
      <button type="submit" class="form__btn">Создать</button>
      <p class="form__sub-text">* - поля обязательные для заполнения</p>
    </div>
    <InvalidFormTextError v-if="isValidForm === null ? false : !isValidForm" />
  </form>
</template>

<script>
import UserData from "./UserData";
import AddressData from "./AddressData";
import PasportData from "./PasportData";
import InvalidFormTextError from "./InvalidFormTextError";

export default {
  props: ["defValidForm"],
  data() {
    return {
      isValidForm: null,
    };
  },
  methods: {
    createdClient(e) {
      let isUserDataValid = this.$refs.UserData.checkUserData(),
        isAddressDataValid = this.$refs.AddressData.checkAddressData(),
        isPasportDataValid = this.$refs.PasportData.checkPasportData();

      if (isUserDataValid && isAddressDataValid && isPasportDataValid) {
        this.$refs.UserData.checkUserData(true);
        this.$refs.PasportData.checkPasportData(true);
        this.$refs.AddressData.checkAddressData(true);

        this.isValidForm = true;
        this.defValidForm(true);

        setTimeout(() => {
          this.isValidForm = null;
          this.defValidForm(null);
        }, 3000);
      } else {
        this.isValidForm = false;
        this.defValidForm(false);
      }
    },
    noNumber(e) {
      let regex = new RegExp("^[0-9]$");
      let key = String.fromCharCode(!e.charCode ? e.which : e.charCode);

      if (regex.test(key)) {
        e.preventDefault();
        return false;
      }
    },
    onlyNumber(e) {
      let regex = new RegExp("^[0-9]$");
      let key = String.fromCharCode(!e.charCode ? e.which : e.charCode);

      if (!regex.test(key)) {
        e.preventDefault();
        return false;
      }
    },
  },
  components: {
    UserData,
    AddressData,
    PasportData,
    InvalidFormTextError,
  },
};
</script>