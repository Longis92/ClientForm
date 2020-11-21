<template>
  <form class="clientForm" @submit.prevent="checkForm">
    <div class="h1">{{ h1 }}</div>
    <div class="form">
      <div class="field">
        <label>Фамилия*</label>
        <input id="secondName" type="text" v-model="secondName" />
        <span v-if="$v.secondName.$invalid" class="errorMessage">Поле фамилия не заполнено</span>
      </div>
      <div class="field">
        <label>Имя*</label>
        <input type="text" v-model="firstName" />
        <span v-if="$v.firstName.$invalid" class="errorMessage">Поле имя не заполнено</span>
      </div>
      <div class="field">
        <label>Отчество</label>
        <input type="text" v-model="patronymicName" />
      </div>
      <div class="field">
        <label>Дата рождения*</label>
        <input type="date" v-model="birthDate" />
        <span v-if="$v.birthDate.$invalid" class="errorMessage">Укажите дату рождения</span>
      </div>
      <div class="field">
        <label for="phoneNumber">Номер телефона*</label>
        <input id="phoneNumber" type="tel" placeholder="7 999 999 9999" maxlength="11" v-model="phoneNumber" />
        <!--<span v-if="$v.phoneNumber.$invalid" class="errorMessage">Введите номер телефона</span>--> 
        <span v-if="phoneValid" class="errorMessage">Номер должен начинаться с цифры 7 и содержать 11 цифр</span>
      </div>
      <div class="field">
        <label for="gendere">Пол</label>
        <input name="gender" type="radio" value="male" v-model="gendere" />
        <span class="gender">М</span>
        <input name="gender" type="radio" value="female" v-model="gendere" />
        <span class="gender">Ж</span>
      </div>
      <div class="field">
        <label for="clientsGroup">Группа клиентов*</label>
        <select id="clientsGroup" multiple class="clientsGroup" v-model="cliGroup" > 
          <option v-for="group in groups" :key="group.id" :value="group.id">
            {{ group.name }}
          </option>
        </select>
        <span v-if="$v.cliGroup.$invalid" class="errorMessage">Укажите группу клиента (или несколько)</span>
      </div>
      <div class="field" id="doctors">
        <label for="doctors">Лечащий врач</label>
        <select v-model="doctor">
          <option value="" selected disabled>Выберите лечащего врача</option>
          <option v-for="doctor in doctors" :key="doctor.id" :value="doctor.id">
            {{ doctor.name }}
          </option>
        </select>
      </div>
      <div class="field">
        <label for="noSMS">Не отправлять СМС</label>
        <input id="noSMS" type="checkbox" v-model="noSMS" />
      </div>
      <!--Адрес-->
      <div class="field">
        <label for="index">Индекс</label>
        <input id="index" type="text" />
      </div>
      <div class="field">
        <label for="country">Страна</label>
        <input id="country" type="text" />
      </div>
      <div class="field">
        <label for="region">Область</label>
        <input id="region" type="text" />
      </div>
      <div class="field">
        <label for="city">Город*</label>
        <input id="city" type="text" v-model="city" />
        <span v-if="$v.city.$invalid" class="errorMessage">Введите город</span>
      </div>
      <div class="field">
        <label for="street">Улица</label>
        <input id="street" type="text" />
      </div>
      <div class="field">
        <label for="house">Дом</label>
        <input id="house" type="text" />
      </div>
      <!--Данные удостоверяющего личность документа-->
      <div class="field" id="personalDocument">
        <label for="personalDocument">Тип документа*</label>
        <select v-model="doctype" >
          <option value="" selected disabled>Укажите тип документа</option>
          <option v-for="document in documents" :key="document.id" :value="document.id">
            {{ document.name }}
          </option>
        </select>
        <span v-if="$v.doctype.$invalid" class="errorMessage">Не указан тип документа</span>
      </div>
      <div class="field">
        <label for="serial">Серия</label>
        <input id="serial" type="text" />
      </div>
      <div class="field">
        <label for="number">Номер</label>
        <input id="number" type="text" />
      </div>
      <div class="field">
        <label for="issuedBy">Кем выдан</label>
        <input id="issuedBy" type="text" />
      </div>
      <div class="field">
        <label for="issueDate">Дата выдачи*</label>
        <input id="issueDate" type="date" v-model="issueDate" />
        <span v-if="$v.issueDate.$invalid" class="errorMessage">Укажите дату выдачи документа</span>
      </div>
      <div class="createButton">
      <button type="checkForm" :disabled="$v.$invalid || phoneValid">Создать клиента</button> <!--кнопка будет неактивна если хотя бы одно из двух условий true-->
      <p>*Поле обязательное для заполнения</p>
      </div>
    </div>
  </form>
</template>

<script>
import { validationMixin } from 'vuelidate'
import { required } from 'vuelidate/lib/validators'

export default {
  mixins: [validationMixin],
  data() {
    return {
      h1: 'Форма создания клиента',
      secondName: '',
      firstName: '',
      birthDate: '',
      phoneNumber: '',
      cliGroup: [],
      doctor: '',
      city: '',
      doctype: '',
      issueDate: '',
      noSMS: false,
      gendere: '',

      groups: [
        {name: 'VIP', id: '1'},
        {name: 'Проблемные', id: '2'},
        {name: 'ОМС', id: '3'}
      ],
      doctors: [
        {name: 'Иванов', id: '1'},
        {name: 'Захаров', id: '2'},
        {name: 'Чернышева', id: '3'}
      ],
      documents: [
        {name: 'Паспорт', id: '1'},
        {name: 'Св-во о рождении', id: '2'},
        {name: 'Вод.удостоверение', id: '3'}
      ],
    }
  },
  computed: {
    phoneValid: function() {
      if (this.phoneNumber[0] === '7' && this.phoneNumber.length === 11) {
        return false;
      }
      return true;
    },
  },
  validations: {
    secondName: {
      required,
    },
    firstName: {
      required,
    },
    birthDate: {
      required,
    },
    cliGroup: {
      required,
    },
    city: {
      required,
    },
    doctype: {
      required,
    },
    issueDate: {
      required,
    },
  },
  methods: {
    checkForm() {
      if (!this.$v.$invalid) {
        this.h1 = "Новый клиент успешно создан!"
      }
    },
  }
}
</script>