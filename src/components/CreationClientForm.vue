<template>
  <form class="form" action="" @submit.prevent="submit">
    <h1 class="title-form">Форма создания Клиента</h1>
    <!-- 1 часть -->
    <div class="input-group">
      <div class="input-wrapper">
        <label for="first_name">Фамилия*</label>
        <input
          :class="['input', $v.form.first_name.$error ? 'input-error' : '']"
          id="first_name"
          type="text"
          name="first_name"
          v-model.trim="$v.form.first_name.$model"
        />

        <div class="error" v-if="$v.form.first_name.$dirty">
          {{ getMessageError($v.form.first_name) }}
        </div>
      </div>

      <div class="input-wrapper">
        <label for="last_name">Имя*</label>
        <input
          :class="['input', $v.form.last_name.$error ? 'input-error' : '']"
          id="last_name"
          type="text"
          name="last_name"
          v-model.trim="$v.form.last_name.$model"
        />

        <div class="error" v-if="$v.form.last_name.$dirty">
          {{ getMessageError($v.form.last_name) }}
        </div>
      </div>

      <div class="input-wrapper">
        <label for="surname">Отчество</label>
        <input
          class="input"
          :class="[
            'input',
            $v.form.surname.$error.required ? 'input-error' : '',
          ]"
          id="surname"
          type="text"
          name="surname"
          v-model.trim="$v.form.surname.$model"
        />

        <div class="error" v-if="$v.form.surname.$dirty">
          {{ getMessageError($v.form.surname) }}
        </div>
      </div>

      <div class="input-wrapper">
        <label for="birthday">Дата рождения*</label>
        <input
          :class="['input', $v.form.birthday.$error ? 'input-error' : '']"
          id="birthday"
          type="date"
          name="birthday"
          v-model="$v.form.birthday.$model"
        />

        <div class="error" v-if="$v.form.birthday.$dirty">
          {{ getMessageError($v.form.birthday) }}
        </div>
      </div>

      <div class="input-wrapper">
        <label for="phone">Номер телефона*</label>
        <input
          :class="['input', $v.form.phone.$error ? 'input-error' : '']"
          id="phone"
          type="tel"
          name="phone"
          maxlength="11"
          placeholder="7**********"
          v-model="$v.form.phone.$model"
          @input="enforcePhoneFormat()"
        />

        <div class="error" v-if="$v.form.phone.$dirty">
          {{ getMessageError($v.form.phone) }}
        </div>
      </div>

      <div class="input-wrapper">
        <label>Пол</label>
        <div class="gender">
          <div>
            <input
              class="radio"
              type="radio"
              id="male"
              name="gender"
              value="male"
              v-model="form.gender"
            />
            <label for="male">Мужчина</label>
          </div>
          <div>
            <input
              class="radio"
              type="radio"
              id="female"
              name="gender"
              value="female"
              v-model="form.gender"
            />
            <label for="female">Женщина</label>
          </div>
        </div>
      </div>

      <div class="input-wrapper">
        <label for="client_group">Группа клиентов*</label>
        <MultipleSelect
          for="client_group"
          :client_group="client_group"
          @selectClientGroups="(res) => (form.selected_client_groups = res)"
        />

        <div
          class="error"
          v-if="
            is_error_select_client && form.selected_client_groups.length === 0
          "
        >
          Ничего не выбрано
        </div>
      </div>

      <div class="input-wrapper">
        <label for="doctor">Лечащий врач</label>
        <select class="select" id="doctor" v-model="form.selected_doctor">
          <option value="" disabled selected>-- Выберите врача --</option>
          <option
            class="option"
            v-for="(doc, index) in doctor"
            :key="index"
            :value="doc"
          >
            {{ doc }}
          </option>
        </select>
      </div>

      <div class="checkbox-wrapper">
        <input
          class="checkbox"
          type="checkbox"
          id="sending_sms"
          v-model="form.no_sending_sms"
        />
        <label for="sending_sms">Не отправлять СМС</label>
      </div>
    </div>

    <!-- 2 часть -->
    <p class="title-part-form">Адрес:</p>
    <div class="input-group">
      <div class="input-wrapper">
        <label for="address_index">Индекс</label>
        <input
          :class="[
            'input',
            $v.form.address.index.$error.required ? 'input-error' : '',
          ]"
          id="address_index"
          type="text"
          maxlength="6"
          name="address_index"
          v-model.trim="$v.form.address.index.$model"
        />

        <div class="error" v-if="$v.form.address.index.$dirty">
          {{ getMessageError($v.form.address.index) }}
        </div>
      </div>

      <div class="input-wrapper">
        <label for="country">Страна</label>
        <input
          :class="[
            'input',
            $v.form.address.country.$error.required ? 'input-error' : '',
          ]"
          id="country"
          type="text"
          name="country"
          v-model.trim="$v.form.address.country.$model"
        />

        <div class="error" v-if="$v.form.address.country.$dirty">
          {{ getMessageError($v.form.address.country) }}
        </div>
      </div>

      <div class="input-wrapper">
        <label for="region">Область</label>
        <input
          :class="[
            'input',
            $v.form.address.region.$error.required ? 'input-error' : '',
          ]"
          id="region"
          type="text"
          name="region"
          v-model.trim="$v.form.address.region.$model"
        />

        <div class="error" v-if="$v.form.address.region.$dirty">
          {{ getMessageError($v.form.address.region) }}
        </div>
      </div>

      <div class="input-wrapper">
        <label for="city">Город*</label>
        <input
          :class="['input', $v.form.address.city.$error ? 'input-error' : '']"
          id="city"
          type="text"
          name="city"
          v-model.trim="$v.form.address.city.$model"
        />

        <div class="error" v-if="$v.form.address.city.$dirty">
          {{ getMessageError($v.form.address.city) }}
        </div>
      </div>

      <div class="input-wrapper">
        <label for="street">Улица</label>
        <input
          :class="[
            'input',
            $v.form.address.street.$error.required ? 'input-error' : '',
          ]"
          id="street"
          type="text"
          name="street"
          v-model.trim="$v.form.address.street.$model"
        />

        <div class="error" v-if="$v.form.address.street.$dirty">
          {{ getMessageError($v.form.address.street) }}
        </div>
      </div>

      <div class="input-wrapper">
        <label for="house">Дом</label>
        <input
          class="input"
          id="house"
          type="text"
          name="house"
          v-model.trim="form.address.house"
        />
      </div>
    </div>

    <!-- 3 часть -->
    <p class="title-part-form">Паспорт:</p>
    <div class="input-group input-group-last">
      <div class="input-wrapper">
        <label for="document_type">Тип документа*</label>
        <select
          class="select"
          id="document_type"
          v-model="form.document.selected_type"
        >
          <option value="" disabled selected>-- Выберите тип --</option>
          <option
            v-for="(type, index) in type_document"
            :key="index"
            :value="type"
          >
            {{ type }}
          </option>
        </select>

        <div
          class="error"
          v-if="is_error_select_type_doc && form.document.selected_type === ''"
        >
          Ничего не выбрано
        </div>
      </div>

      <div class="input-wrapper">
        <label for="series">Серия</label>
        <input
          class="input"
          id="series"
          type="text"
          name="series"
          v-model.trim="form.document.series"
        />
      </div>

      <div class="input-wrapper">
        <label for="number">Номер</label>
        <input
          :class="[
            'input',
            $v.form.document.number.$error.required ? 'input-error' : '',
          ]"
          id="number"
          type="text"
          maxlength="6"
          name="number"
          v-model.trim="$v.form.document.number.$model"
        />

        <div class="error" v-if="$v.form.document.number.$dirty">
          {{ getMessageError($v.form.document.number) }}
        </div>
      </div>

      <div class="input-wrapper">
        <label for="issued">Кем выдан</label>
        <input
          class="input"
          id="issued"
          type="text"
          name="issued"
          v-model.trim="form.document.issued"
        />
      </div>

      <div class="input-wrapper">
        <label for="issue_date">Дата выдачи*</label>
        <input
          :class="[
            'input',
            $v.form.document.issue_date.$error ? 'input-error' : '',
          ]"
          id="issue_date"
          type="date"
          name="issue_date"
          v-model="$v.form.document.issue_date.$model"
        />

        <div class="error" v-if="$v.form.document.issue_date.$dirty">
          {{ getMessageError($v.form.document.issue_date) }}
        </div>
      </div>
    </div>

    <input class="create-btn" type="submit" value="Создать" />

    <p class="notification">* Поле обязательное для заполнения.</p>
  </form>
</template>

<script>
import { validationMixin } from "vuelidate";
import { required, minLength, maxLength } from "vuelidate/lib/validators";

import MultipleSelect from "@/components/MultipleSelect.vue";

export default {
  name: "CreationClientForm",
  components: { MultipleSelect },
  mixins: [validationMixin],

  data() {
    return {
      form: {
        first_name: "",
        last_name: "",
        surname: "",
        birthday: "",
        phone: "",
        gender: "male",
        selected_client_groups: [],
        selected_doctor: "",
        no_sending_sms: false,
        address: {
          index: "",
          country: "",
          region: "",
          city: "",
          street: "",
          house: "",
        },
        document: {
          selected_type: "",
          series: "",
          number: "",
          issued: "",
          issue_date: "",
        },
      },

      client_group: ["VIP", "Проблемные", "ОМС"],
      doctor: ["Иванов", "Захаров", "Чернышева"],
      type_document: [
        "Паспорт",
        "Свидетельство о рождении",
        "Вод. удостоверение",
      ],

      is_error_select_client: false,
      is_error_select_type_doc: false,
      is_success_form: false,
    };
  },

  validations: {
    form: {
      first_name: {
        required,
        minLength: minLength(2),
        checkField(value) {
          return this.checkInputName(value);
        },
      },
      last_name: {
        required,
        minLength: minLength(2),
        checkField(value) {
          return this.checkInputName(value);
        },
      },
      surname: {
        checkField(value) {
          return this.checkInputName(value);
        },
      },
      birthday: {
        required,
        maxLength: maxLength(10),
        checkField(value) {
          return this.checkDate(value);
        },
      },
      phone: {
        required,
        minLength: minLength(11),
        checkField(value) {
          return +value[0] === 7;
        },
      },
      address: {
        index: {
          checkField(value) {
            let reg = /^\d+$/;
            return reg.test(value) && value.length === 6;
          },
        },
        country: {
          checkField(value) {
            return this.checkInputAddress(value);
          },
        },
        region: {
          checkField(value) {
            return this.checkInputAddress(value);
          },
        },
        city: {
          required,
          minLength: minLength(2),
          checkField(value) {
            return this.checkInputAddress(value);
          },
        },
        street: {
          checkField(value) {
            return this.checkInputAddress(value);
          },
        },
      },
      document: {
        number: {
          checkField(value) {
            let reg = /^\d+$/;
            return reg.test(value) && value.length === 6;
          },
        },
        issue_date: {
          required,
          maxLength: maxLength(10),
          checkField(value) {
            return this.checkDate(value);
          },
        },
      },
    },
  },

  methods: {
    getMessageError(error) {
      if (error.required !== undefined && !error.required)
        return "Обязательное поле";
      else if (error.minLength !== undefined && !error.minLength)
        return "Некорректное поле";
      else if (error.maxLength !== undefined && !error.maxLength)
        return "Некорректное поле";
      else if (
        error.$model !== "" &&
        error.checkField !== undefined &&
        !error.checkField
      )
        return "Некорректное поле";
    },
    enforcePhoneFormat() {
      this.form.phone = this.form.phone.replace(/[^0-9]/g, "");
      this.form.phone = this.form.phone.replace(/^[8]/g, "7");
    },
    checkInputName(value) {
      let reg = /^[a-zA-Zа-яА-Я-\s]+$/;
      return reg.test(value);
    },
    checkDate(value) {
      let mas_date = value.split("-");
      let value_year = +mas_date[0];
      let value_month = +mas_date[1] - 1;
      let value_day = +mas_date[2];
      let value_date = new Date(value_year, value_month, value_day);
      let now_year = new Date().getFullYear();
      return value_year >= now_year - 150 && value_date < new Date();
    },
    checkInputAddress(value) {
      let reg = /^[a-zA-Zа-яА-Я-.,\s]+$/;
      return reg.test(value);
    },
    checkMultSelectEmpty() {
      if (this.form.selected_client_groups.length === 0)
        this.is_error_select_client = true;
      else this.is_error_select_client = false;
    },
    checkSelectEmpty() {
      if (this.form.document.selected_type === "")
        this.is_error_select_type_doc = true;
      else this.is_error_select_type_doc = false;
    },
    checkNoRequiredField() {
      let flag = [];
      if (
        this.form.surname === "" ||
        (this.form.surname !== "" && this.$v.form.surname.checkField)
      )
        flag.push(true);
      else flag.push(false);
      if (
        this.form.address.index === "" ||
        (this.form.address.index !== "" &&
          this.$v.form.address.index.checkField)
      )
        flag.push(true);
      else flag.push(false);
      if (
        this.form.address.country === "" ||
        (this.form.address.country !== "" &&
          this.$v.form.address.country.checkField)
      )
        flag.push(true);
      else flag.push(false);
      if (
        this.form.address.region === "" ||
        (this.form.address.region !== "" &&
          this.$v.form.address.region.checkField)
      )
        flag.push(true);
      else flag.push(false);
      if (
        this.form.address.street === "" ||
        (this.form.address.street !== "" &&
          this.$v.form.address.street.checkField)
      )
        flag.push(true);
      else flag.push(false);
      if (
        this.form.document.number === "" ||
        (this.form.document.number !== "" &&
          this.$v.form.document.number.checkField)
      )
        flag.push(true);
      else flag.push(false);
      return flag.includes(false);
    },
    checkRequiredField() {
      let flag = [];
      if (
        this.$v.form.first_name.required &&
        this.$v.form.first_name.minLength &&
        this.$v.form.first_name.checkField
      )
        flag.push(true);
      else flag.push(false);
      if (
        this.$v.form.last_name.required &&
        this.$v.form.last_name.minLength &&
        this.$v.form.last_name.checkField
      )
        flag.push(true);
      else flag.push(false);
      if (
        this.$v.form.birthday.required &&
        this.$v.form.birthday.maxLength &&
        this.$v.form.birthday.checkField
      )
        flag.push(true);
      else flag.push(false);
      if (
        this.$v.form.phone.required &&
        this.$v.form.phone.minLength &&
        this.$v.form.phone.checkField
      )
        flag.push(true);
      else flag.push(false);
      if (
        this.$v.form.address.city.required &&
        this.$v.form.address.city.minLength &&
        this.$v.form.address.city.checkField
      )
        flag.push(true);
      else flag.push(false);
      if (
        this.$v.form.document.issue_date.required &&
        this.$v.form.document.issue_date.maxLength &&
        this.$v.form.document.issue_date.checkField
      )
        flag.push(true);
      else flag.push(false);
      return flag.includes(false);
    },
    validationForm() {
      this.$v.form.$touch();
      this.checkMultSelectEmpty();
      this.checkSelectEmpty();

      if (!this.checkNoRequiredField() && !this.checkRequiredField()) {
        if (!this.is_error_select_client && !this.is_error_select_type_doc)
          this.is_success_form = true;
        else this.is_success_form = false;
      } else this.is_success_form = false;

      return this.is_success_form;
    },
    submit() {
      if (this.validationForm()) {
        console.log("Валидация прошла успешно");
        console.log(this.form);
        this.$emit("createdClient");
      } else console.log("Error!");
    },
  },
};
</script>

<style scoped lang="sass">
$tablet: 768px

$firstColor: #00a4af
$secondColor: #aeaeae
$errorColor: #ff0000

.form
  max-width: 1000px
  padding: 20px

.title-form
  font-size: 24px
  text-align: center
  margin-bottom: 50px

.input-group
  display: grid
  grid-template-columns: repeat(3, 1fr)
  gap: 30px
  padding-bottom: 30px
  border-bottom: 1px solid $firstColor

  @media (max-width: $tablet)
    grid-template-columns: repeat(1, 1fr)

.input-group-last
  border-bottom: none

.input-wrapper
  display: flex
  flex-direction: column
  gap: 10px

.input
  all: unset
  padding: 10px
  border: 1px solid $secondColor
  border-radius: 7px

  &:focus
    border: 1px solid $firstColor
  &:focus::-webkit-input-placeholder
    color: transparent
  &:focus::-moz-placeholder
    color: transparent
  &:focus:-moz-placeholder
    color: transparent
  &:focus:-ms-input-placeholder
    color: transparent

.gender
  display: flex
  flex-direction: row
  gap: 20px

.radio
  margin-right: 5px
  accent-color: $firstColor

.select
  padding: 10px
  border: 1px solid $secondColor
  border-radius: 7px

.checkbox-wrapper
  display: flex
  flex-direction: row
  align-items: center
  gap: 5px

.checkbox
  height: 30px
  accent-color: $firstColor

.error
  color: $errorColor
  margin-top: -5px

.input-error
  color: $errorColor
  border: 1px solid $errorColor !important

.title-part-form
  font-size: 18px
  font-weight: 700
  margin-top: 30px

.create-btn
  all: unset
  width: 150px
  padding: 10px
  margin-top: 20px
  font-size: 18px
  text-align: center
  color: #ffffff
  border-radius: 7px
  background-color: $firstColor
  cursor: pointer
  &:hover
    color: #000000
    border: 1px solid $firstColor
    background-color: #ffffff

.notification
  color: rgba(#000000, 0.7)
  margin-top: 25px
</style>
