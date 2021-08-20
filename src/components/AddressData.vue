<template>
  <div class="form__block">
    <div class="form__group">
      <label for="index">Индекс</label>
      <input
        name="index"
        id="index"
        type="text"
        v-model.trim="form.indexData"
        @keypress="onlyNumber($event)"
      />
    </div>
    <div class="form__group">
      <label for="country">Страна</label>
      <input
        name="country"
        id="country"
        type="text"
        v-model.trim="form.countryData"
        @keypress="noNumber($event)"
      />
    </div>
    <div class="form__group">
      <label for="region">Регион</label>
      <input
        name="region"
        id="region"
        type="text"
        v-model.trim="form.regionData"
        @keypress="noNumber($event)"
      />
    </div>
    <div class="form__group">
      <label for="city">Город<span>*</span></label>
      <input
        name="city"
        id="city"
        type="text"
        :class="$v.form.cityData.$error ? 'is-invalid' : ''"
        @blur="$v.form.cityData.$touch()"
        v-model.trim="form.cityData"
        @keypress="noNumber($event)"
      />
      <RequiredTextError
        v-if="$v.form.cityData.$dirty && !$v.form.cityData.required"
      />
    </div>
    <div class="form__group">
      <label for="street">Улица</label>
      <input
        name="street"
        id="street"
        type="text"
        v-model.trim="form.streetData"
      />
    </div>
    <div class="form__group">
      <label for="house ">Дом</label>
      <input
        name="house "
        id="house"
        type="text"
        v-model.trim="form.houseData"
      />
    </div>
  </div>
</template>

<script>
import { validationMixin } from "vuelidate";
import { required } from "vuelidate/lib/validators";
import RequiredTextError from "./RequiredTextError";

export default {
  props: {
    onlyNumber: Function,
    noNumber: Function,
  },
  mixins: [validationMixin],
  data() {
    return {
      form: {
        indexData: "",
        countryData: "",
        regionData: "",
        cityData: null,
        streetData: "",
        houseData: "",
      },
    };
  },
  validations: {
    form: {
      cityData: {
        required,
      },
    },
  },
  methods: {
    checkAddressData(isValidForm = false) {
      if (isValidForm) {
        this.form.indexData = "";
        this.form.countryData = "";
        this.form.regionData = "";
        this.form.cityData = null;
        this.form.streetData = "";
        this.form.houseData = "";

        setTimeout(() => {
          this.$v.form.cityData.$reset();
        }, 0);
      } else {
        this.$v.form.cityData.$touch();

        if (!this.$v.form.cityData.$error) {
          return true;
        } else {
          return false;
        }
      }
    },
  },
  components: {
    RequiredTextError,
  },
};
</script>