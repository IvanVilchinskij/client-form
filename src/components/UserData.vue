<template>
  <div class="form__block">
    <h2 class="form__title"><span>Шаг 1</span> / Личные данные</h2>
    <div class="form__group">
      <input
        name="surname"
        id="surname"
        type="text"
        placeholder="Фамилия*"
        :class="$v.form.surnameData.$error ? 'is-invalid' : ''"
        v-model.trim="form.surnameData"
        @blur="$v.form.surnameData.$touch()"
        @keypress="noNumber($event)"
      />
      <RequiredTextError
        v-if="$v.form.surnameData.$dirty && !$v.form.surnameData.required"
      />
    </div>
    <div class="form__group">
      <input
        name="name"
        id="name"
        type="text"
        placeholder="Имя*"
        :class="$v.form.nameData.$error ? 'is-invalid' : ''"
        v-model.trim="form.nameData"
        @blur="$v.form.nameData.$touch()"
        @keypress="noNumber($event)"
      />
      <RequiredTextError
        v-if="$v.form.nameData.$dirty && !$v.form.nameData.required"
      />
    </div>
    <div class="form__group">
      <input
        name="patronymic"
        id="patronymic"
        type="text"
        placeholder="Отчество"
        v-model.trim="form.patronymicData"
        @keypress="noNumber($event)"
      />
    </div>
    <div class="form__group">
      <label for="birthday">Дата рождения*</label>
      <input
        name="birthday"
        id="birthday"
        v-model="form.birthdayData"
        :class="$v.form.birthdayData.$error ? 'is-invalid' : ''"
        type="date"
        @blur="$v.form.birthdayData.$touch()"
      />
      <RequiredTextError
        v-if="$v.form.birthdayData.$dirty && !$v.form.birthdayData.required"
      />
    </div>
    <div class="form__group">
      <input
        name="tel"
        id="tel"
        type="text"
        placeholder="Телефон"
        :class="$v.form.telData.$error ? 'is-invalid' : ''"
        v-model.trim="form.telData"
        @keypress="onlyNumber($event)"
        @click="addFirstNumber($event)"
        @blur="$v.form.telData.$touch()"
      />
      <MinLengthTextError
        v-if="
          $v.form.telData.$dirty &&
          (!$v.form.telData.minLength || !$v.form.telData.maxLength)
        "
      />
    </div>
    <div class="form__group">
      <div class="form__group-radio">
        <input
          name="gendere"
          id="male"
          type="radio"
          value="male"
          v-model="form.gendere"
        />
        <label for="male">Мужчина</label>
        <input
          name="gendere"
          id="female"
          type="radio"
          value="female"
          v-model="form.gendere"
        />
        <label for="female">Женщина</label>
      </div>
    </div>
    <div class="form__group">
      <label for="clients">Группа клиентов*</label>
      <select
        name="clients"
        id="clients"
        v-model="form.clientData"
        :class="$v.form.clientData.$error ? 'is-invalid' : ''"
        multiple
        @blur="$v.form.clientData.$touch()"
      >
        <option v-for="(client, i) in clients" :key="i" :value="client.value">
          {{ client.label }}
        </option>
      </select>
      <RequiredTextError
        v-if="$v.form.clientData.$dirty && !$v.form.clientData.required"
      />
    </div>
    <div class="form__group">
      <label for="doctor">Лечащий врач</label>
      <select name="doctor" id="doctor" v-model="form.doctorData">
        <option v-for="(doctor, i) in doctors" :key="i" :value="doctor.value">
          {{ doctor.label }}
        </option>
      </select>
    </div>
    <div class="form__group form__group--sms">
      <input name="SMS" id="SMS" type="checkbox" v-model="form.agreeSMS" />
      <label for="SMS">Не отправлять СМС</label>
    </div>
    <p class="form__sub-text">* - поля обязательные для заполнения</p>
    <div class="form__bottom">
      <button @click.prevent="nextStep" class="form__next">Далее</button>
    </div>
  </div>
</template>

<script>
import { validationMixin } from "vuelidate";
import { required, minLength, maxLength } from "vuelidate/lib/validators";
import RequiredTextError from "./RequiredTextError";
import MinLengthTextError from "./MinLengthTextError";

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
        surnameData: null,
        nameData: null,
        patronymicData: "",
        birthdayData: null,
        telData: "",
        doctorData: "",
        clientData: [],
        agreeSMS: false,
        gendere: "male",
      },
      isChangeColorDate: false,
      doctors: [
        {
          label: "Иванов",
          value: "Ivanov",
        },
        {
          label: "Захаров",
          value: "Zaharov",
        },
        {
          label: "Чернышева",
          value: "Chernishova",
        },
      ],
      clients: [
        {
          label: "VIP",
          value: "VIP",
        },
        {
          label: "Проблемные",
          value: "problem",
        },
        {
          label: "ОМС",
          value: "OMS",
        },
      ],
    };
  },
  validations: {
    form: {
      surnameData: {
        required,
      },
      nameData: {
        required,
      },
      birthdayData: {
        required,
      },
      telData: {
        minLength: minLength(11),
        maxLength: maxLength(11),
      },
      clientData: {
        required,
      },
    },
  },
  methods: {
    addFirstNumber(e) {
      const target = e.target;

      if (target.value.length === 0) {
        this.form.telData = 7;
      }
    },
    checkUserData(isValidForm = false) {
      if (isValidForm) {
        this.form.surnameData = null;
        this.form.nameData = null;
        this.form.patronymicData = "";
        this.form.birthdayData = null;
        this.form.telData = "";
        this.form.doctorData = "";
        this.form.clientData = [];
        this.form.agreeSMS = false;
        this.form.gendere = "male";

        setTimeout(() => {
          this.$v.form.surnameData.$reset();
          this.$v.form.nameData.$reset();
          this.$v.form.birthdayData.$reset();
          this.$v.form.clientData.$reset();
          this.$v.form.telData.$reset();
        }, 0);
      } else {
        this.$v.form.surnameData.$touch();
        this.$v.form.nameData.$touch();
        this.$v.form.birthdayData.$touch();
        this.$v.form.clientData.$touch();

        if (
          !this.$v.form.surnameData.$error &&
          !this.$v.form.nameData.$error &&
          !this.$v.form.birthdayData.$error &&
          !this.$v.form.clientData.$error
        ) {
          return true;
        } else {
          return false;
        }
      }
    },
    nextStep() {
      if (this.checkUserData()) {
        this.addToFormData(this.form);
        this.changeFormStep(2);
      }
    },
  },

  components: {
    RequiredTextError,
    MinLengthTextError,
  },
};
</script>