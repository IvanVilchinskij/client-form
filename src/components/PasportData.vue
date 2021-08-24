<template>
  <div class="form__block">
    <h2 class="form__title"><span>Шаг 3</span> / Паспорт</h2>
    <div class="form__group">
      <label for="doctype">Тип документа*</label>
      <select
        name="doctype"
        id="doctype"
        v-model.trim="form.doctypeData"
        :class="$v.form.doctypeData.$error ? 'is-invalid' : ''"
        @blur="$v.form.doctypeData.$touch()"
      >
        <option v-for="(item, i) in doctypes" :key="i" :value="item.value">
          {{ item.label }}
        </option>
      </select>
      <RequiredTextError
        v-if="$v.form.doctypeData.$dirty && !$v.form.doctypeData.required"
      />
    </div>
    <div class="form__group">
      <input
        name="serie"
        id="serie"
        type="text"
        placeholder="Серия"
        v-model.trim="form.serieData"
        @keypress="onlyNumber($event)"
      />
    </div>
    <div class="form__group">
      <input
        name="number"
        id="number"
        type="text"
        placeholder="Номер"
        v-model.trim="form.numberData"
        @keypress="onlyNumber($event)"
      />
    </div>
    <div class="form__group">
      <input
        name="issued-by"
        id="issued-by"
        type="text"
        placeholder="Кем выдан"
        v-model.trim="form.issuedByData"
      />
    </div>
    <div class="form__group">
      <label for="issued-date">Дата выдачи*</label>
      <input
        name="issued-date"
        id="issued-date"
        type="date"
        :class="$v.form.issuedDateData.$error ? 'is-invalid' : ''"
        @blur="$v.form.issuedDateData.$touch()"
        v-model.trim="form.issuedDateData"
      />
      <RequiredTextError
        v-if="$v.form.issuedDateData.$dirty && !$v.form.issuedDateData.required"
      />
    </div>
    <p class="form__sub-text">* - поля обязательные для заполнения</p>

    <div class="form__bottom">
      <button @click.prevent="prevStep" class="form__prev">Назад</button>
      <button type="submit" @click="addToFormData(form)" class="form__btn">
        Готово
      </button>
      <InvalidFormTextError
        v-if="isValidForm === null ? false : !isValidForm"
      />
    </div>
  </div>
</template>

<script>
import { validationMixin } from "vuelidate";
import { required } from "vuelidate/lib/validators";
import RequiredTextError from "./RequiredTextError";
import InvalidFormTextError from "../components/InvalidFormTextError";

export default {
  props: {
    onlyNumber: Function,
    noNumber: Function,
    isValidForm: Boolean,
    addToFormData: Function,
    changeFormStep: Function,
  },
  mixins: [validationMixin],
  data() {
    return {
      form: {
        doctypeData: null,
        serieData: "",
        numberData: "",
        issuedByData: "",
        issuedDateData: null,
      },
      doctypes: [
        {
          label: "Паспорт",
          value: "passport",
        },
        {
          label: "Свидетельство о рождении",
          value: "birthday-document",
        },
        {
          label: "Вод. удостоверение",
          value: "driver-document",
        },
      ],
    };
  },
  validations: {
    form: {
      doctypeData: {
        required,
      },
      issuedDateData: {
        required,
      },
    },
  },
  methods: {
    checkPasportData(isValidForm = false) {
      if (isValidForm) {
        this.form.doctypeData = null;
        this.form.serieData = "";
        this.form.numberData = "";
        this.form.issuedByData = "";
        this.form.issuedDateData = null;

        setTimeout(() => {
          this.$v.form.doctypeData.$reset();
          this.$v.form.issuedDateData.$reset();
        }, 0);
      } else {
        this.$v.form.doctypeData.$touch();
        this.$v.form.issuedDateData.$touch();

        if (
          !this.$v.form.doctypeData.$error &&
          !this.$v.form.issuedDateData.$error
        ) {
          return true;
        } else {
          return false;
        }
      }
    },
    prevStep() {
      this.changeFormStep(2);
    },
  },
  components: {
    RequiredTextError,
    InvalidFormTextError,
  },
};
</script>