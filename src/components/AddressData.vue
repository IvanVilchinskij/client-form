<template>
  <div class="form__block">
    <h2 class="form__title"><span>Шаг 2</span> / Адрес</h2>
    <div class="form__group">
      <input
        name="index"
        id="index"
        type="text"
        placeholder="Индекс"
        v-model.trim="form.indexData"
        @keypress="onlyNumber($event)"
      />
    </div>
    <div class="form__group">
      <input
        name="country"
        id="country"
        type="text"
        placeholder="Страна"
        v-model.trim="form.countryData"
        @keypress="noNumber($event)"
      />
    </div>
    <div class="form__group">
      <input
        name="region"
        id="region"
        type="text"
        placeholder="Регион"
        v-model.trim="form.regionData"
        @keypress="noNumber($event)"
      />
    </div>
    <div class="form__group">
      <input
        name="city"
        id="city"
        type="text"
        placeholder="Город*"
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
      <input
        name="street"
        id="street"
        type="text"
        placeholder="Улица"
        v-model.trim="form.streetData"
      />
    </div>
    <div class="form__group">
      <input
        name="house "
        id="house"
        type="text"
        placeholder="Дом"
        v-model.trim="form.houseData"
      />
    </div>
    <p class="form__sub-text">* - поля обязательные для заполнения</p>
    <div class="form__bottom">
      <button @click.prevent="prevStep" class="form__prev">Назад</button>
      <button @click.prevent="nextStep" class="form__next">Далее</button>
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
    addToFormData: Function,
    changeFormStep: Function,
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
    nextStep() {
      if (this.checkAddressData()) {
        this.addToFormData(this.form);
        this.changeFormStep(3);
      }
    },
    prevStep() {
      this.changeFormStep(1);
    },
  },
  components: {
    RequiredTextError,
  },
};
</script>