<template>
  <div class="form">
    {{ formData }}
    <form action="" autocomplete="false" @submit.prevent="onFormSubmit">
      <h2>Личные данные</h2>
      <div class="row">
        <label for="lastName" class="block">Фамилия</label>
        <input id="lastName" v-model="formData.lastName" />
      </div>

      <div class="row">
        <label for="firstName" class="block">Имя</label>
        <input id="firstName" v-model="formData.firstName" />
      </div>

      <div class="row">
        <label for="middleName" class="block">Отчество</label>
        <input id="middleName" v-model="formData.middleName" />
      </div>

      <div class="row">
        <label for="birthDate" class="block">Дата рождения</label>
        <input
          id="birthDate"
          placeholder="дд.мм.гггг"
          v-model="formData.birthDate"
        />
      </div>

      <div class="row">
        <label for="email" class="block">E-mail</label>
        <input id="email" v-model="formData.email" />
      </div>

      <div class="row">
        <label class="block">Пол</label>
        <input
          type="radio"
          name="gender"
          id="gender_male"
          value="male"
          v-model="formData.gender"
        />
        <label for="gender_male"> Мужской </label>
        <input
          type="radio"
          name="gender"
          id="gender_female"
          value="female"
          v-model="formData.gender"
        />
        <label for="gender_female"> Женский </label>
      </div>

      <!---------  PASSPORT INFORMATION --------->
      <h2>Паспортные данные</h2>
      <div class="row" v-click-outside="hideNationalityDropdown">
        <label for="nationality" class="block">Гражданство</label>
        <input
          id="nationality"
          v-model="nationalityFilterText"
          @focus="onNationalityFilterFocus"
        />

        <div v-if="isNationalityOpen" class="selector_dropdown">
          <ul>
            <li
              v-for="citizenship in filteredCitizenships"
              :key="citizenship.id"
              @click="onCitizenshipClicked(citizenship)"
            >
              {{ citizenship.nationality }}
            </li>
          </ul>
        </div>
      </div>

      <div v-if="isRussianNationality">
        <div class="row">
          <label for="russianPassportSeries" class="block"
            >Серия паспорта</label
          >
          <input
            id="russianPassportSeries"
            v-model="formData.russianPassport.passportSeries"
          />
        </div>

        <div class="row">
          <label for="russianPassportNumber" class="block"
            >Номер паспорта</label
          >
          <input
            id="russianPassportNumber"
            v-model="formData.russianPassport.passportNumber"
          />
        </div>

        <div class="row">
          <label for="russianPassportNumber" class="block">Дата выдачи</label>
          <input
            id="russianPassportIssueDate"
            placeholder="дд.мм.гггг"
            v-model="formData.russianPassport.issueDate"
          />
        </div>
      </div>

      <div v-else-if="isForeignNationality">
        <div class="row">
          <label for="foreignPassportLastName" class="block"
            >Фамилия на латиннице</label
          >
          <input
            id="foreignPassportFirstName"
            v-model="formData.foreignPassport.latinLastName"
          />
        </div>

        <div class="row">
          <label for="foreignPassportFirstName" class="block"
            >Имя на латиннице</label
          >
          <input
            id="foreignPassportFirstName"
            v-model="formData.foreignPassport.latinFirstName"
          />
        </div>

        <div class="row">
          <label for="foreignPassportNumber" class="block"
            >Номер паспорта</label
          >
          <input
            id="foreignPassportNumber"
            v-model="formData.foreignPassport.passportNumber"
          />
        </div>

        <div class="row">
          <label for="foreignPassportCountry" class="block"
            >Страна выдачи</label
          >
          <select id="foreignPassportCountry">
            <option
              v-for="citizenship in citizenships"
              :key="citizenship.id"
              value="citizenship.nationality"
            >
              {{ citizenship.nationality }}
            </option>
          </select>
        </div>

        <div class="row">
          <label for="foreignPassportType" class="block">Тип паспорта</label>
          <select name="foreignPassportType" id="foreignPassportType">
            <option
              v-for="passportType in passportTypes"
              :key="passportType.id"
              value="passportType.type"
            >
              {{ passportType.type }}
            </option>
          </select>
        </div>
      </div>

      <!---------  NAME CHANGE INFORMATION --------->
      <h2>Меняли ли фамилию или имя?</h2>
      <div class="row">
        <input
          type="radio"
          name="wasNameChanged"
          id="nameWasNotChanged"
          :value="false"
          v-model="formData.wasNameChanged"
        />
        <label for="nameWasNotChanged"> Нет </label>
        <input
          type="radio"
          name="wasNameChanged"
          id="nameWasChanged"
          :value="true"
          v-model="formData.wasNameChanged"
        />
        <label for="nameWasChanged"> Да </label>
      </div>
      <div class="row" v-if="formData.wasNameChanged">
        <label class="block" for="previousLastName">Фамилия</label>
        <input id="previousLastName" v-model="formData.previousName.lastName" />
        <label class="block" for="previousFirstName">Имя</label>
        <input
          id="previousLastName"
          v-model="formData.previousName.firstName"
        />
      </div>

      <button>Отправить</button>
    </form>
  </div>
</template>

<script>
import ClickOutside from 'vue-click-outside';
import citizenships from '@/assets/data/citizenships.json';
import passportTypes from '@/assets/data/passport-types.json';

export default {
  data() {
    return {
      formData: {
        lastName: '',
        firstName: '',
        middleName: '',
        birthDate: '',
        email: '',
        gender: '',
        nationality: '',
        russianPassport: {
          passportSeries: '',
          passportNumber: '',
          issueDate: '',
        },
        foreignPassport: {
          latinLastName: '',
          latinFirstName: '',
          passportNumber: '',
          passportCountry: '',
          passportType: '',
        },
        wasNameChanged: false,
        previousName: {
          lastName: '',
          firstName: '',
        },
      },
      isNationalityOpen: false,
      nationalityFilterText: '',
      citizenships,
      passportTypes,
    };
  },
  computed: {
    filteredCitizenships() {
      return this.citizenships.filter((it) =>
        it.nationality.includes(this.nationalityFilterText)
      );
    },
    isRussianNationality() {
      return this.formData.nationality === 'Russia';
    },
    isForeignNationality() {
      return !this.isRussianNationality && this.formData.nationality.length > 0;
    },
    wasNameChanged() {
      return this.formData.wasNameChanged;
    },
  },
  methods: {
    onNationalityFilterFocus() {
      this.nationalityFilterText = '';
      this.isNationalityOpen = true;
    },
    onCitizenshipClicked(selectedCitizenship) {
      this.formData.nationality = selectedCitizenship.nationality;
      this.nationalityFilterText = this.formData.nationality;
      this.hideNationalityDropdown();
    },
    hideNationalityDropdown() {
      this.isNationalityOpen = false;
    },
    onFormSubmit() {
      const prettifiedData = JSON.stringify(this.formData, null, 2);
      console.log(prettifiedData);
    },
  },
  directives: {
    ClickOutside,
  },
};
</script>

<style scoped>
.row {
  margin: 20px 0;
}
.block {
  display: block;
}

.selector_dropdown {
  width: 200px;
  max-height: 300px;
  overflow-y: scroll;
  border: 1px solid black;
  border-radius: 5px;
  cursor: pointer;
  line-height: 1.3;
}

.selector_dropdown ul {
  padding: 0;
  margin: 0;
}

.selector_dropdown li {
  display: block;
  margin: 0;
  padding: 3px;
}

.selector_dropdown li:hover {
  background: #d3e1eb;
}
</style>
