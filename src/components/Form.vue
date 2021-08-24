<template>
  <form class="form" @submit.prevent="createdClient">
    <UserData
      v-show="steps[1]"
      ref="UserData"
      :noNumber="noNumber"
      :onlyNumber="onlyNumber"
      :addToFormData="addToFormData"
      :changeFormStep="changeFormStep"
    />
    <AddressData
      v-show="steps[2]"
      ref="AddressData"
      :noNumber="noNumber"
      :onlyNumber="onlyNumber"
      :addToFormData="addToFormData"
      :changeFormStep="changeFormStep"
    />
    <PasportData
      v-show="steps[3]"
      ref="PasportData"
      :noNumber="noNumber"
      :onlyNumber="onlyNumber"
      :isValidForm="isValidForm"
      :addToFormData="addToFormData"
      :changeFormStep="changeFormStep"
    />
  </form>
</template>

<script>
import UserData from "./UserData";
import AddressData from "./AddressData";
import PasportData from "./PasportData";

export default {
  props: ["defValidForm"],
  data() {
    return {
      isValidForm: null,
      steps: {
        1: true,
        2: false,
        3: false,
      },
      formData: {
        surnameData: null,
        nameData: null,
        patronymicData: "",
        birthdayData: null,
        telData: "7",
        doctorData: "",
        clientData: [],
        agreeSMS: false,
        gendere: "male",
        indexData: "",
        countryData: "",
        regionData: "",
        cityData: null,
        streetData: "",
        houseData: "",
        doctypeData: null,
        serieData: "",
        numberData: "",
        issuedByData: "",
        issuedDateData: null,
      },
    };
  },
  methods: {
    createdClient() {
      let isUserDataValid = this.$refs.UserData.checkUserData(),
        isAddressDataValid = this.$refs.AddressData.checkAddressData(),
        isPasportDataValid = this.$refs.PasportData.checkPasportData();

      if (isUserDataValid && isAddressDataValid && isPasportDataValid) {
        this.$refs.UserData.checkUserData(true);
        this.$refs.PasportData.checkPasportData(true);
        this.$refs.AddressData.checkAddressData(true);

        this.isValidForm = true;
        this.defValidForm(true);

        this.steps = {
          1: true,
          2: false,
          3: false,
        };

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
    addToFormData(data) {
      this.formData = Object.assign(this.formData, data);
    },
    changeFormStep(step) {
      for (let key in this.steps) {
        if (+key !== step) {
          this.steps[key] = false;
        } else {
          this.steps[key] = true;
        }
      }
    },
  },
  components: {
    UserData,
    AddressData,
    PasportData,
  },
};
</script>