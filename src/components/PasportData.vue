<template>
  <div class="form__block">
    <div class="form__group">
      <label for="doctype">Тип документа<span>*</span></label>
      <select
        name="doctype"
        id="doctype"
        v-model.trim="form.doctypeData"
        :class="$v.form.doctypeData.$error ? 'is-invalid' : ''"
        @blur="$v.form.doctypeData.$touch()"
      >
        <option v-for="(item, i) in doctypes" :key="i" value="item.value">
          {{ item.label }}
        </option>
      </select>
      <RequiredTextError
        v-if="$v.form.doctypeData.$dirty && !$v.form.doctypeData.required"
      />
    </div>
    <div class="form__group">
      <label for="serie">Серия</label>
      <input
        name="serie"
        id="serie"
        type="text"
        v-model.trim="form.serieData"
        @keypress="onlyNumber($event)"
      />
    </div>
    <div class="form__group">
      <label for="number">Номер</label>
      <input
        name="number"
        id="number"
        type="text"
        v-model.trim="form.numberData"
        @keypress="onlyNumber($event)"
      />
    </div>
    <div class="form__group">
      <label for="issued-by">Кем выдан</label>
      <input
        name="issued-by"
        id="issued-by"
        type="text"
        v-model.trim="form.issuedByData"
      />
    </div>
    <div class="form__group">
      <label for="issued-date">Дата выдачи<span>*</span></label>
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
  },
  components: {
    RequiredTextError,
  },
};
</script>