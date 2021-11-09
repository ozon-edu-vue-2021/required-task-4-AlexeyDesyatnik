<template>
  <div class="form">
    {{ formData }}
    <form action="">
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

      <h2>Паспортные данные</h2>
      <div class="row">
        <label for="nationality" class="block">Гражданство</label>
        <input id="nationality" v-model="formData.nationality" />

        <div class="selector_dropdown">
          <ul>
            <li
              v-for="citizenship in citizenships"
              :key="citizenship.id"
              @click="onCitizenshipClicked(citizenship)"
            >
              {{ citizenship.nationality }}
            </li>
          </ul>
        </div>
      </div>

      <button>Отправить</button>
    </form>
  </div>
</template>

<script>
import citizenships from '@/assets/data/citizenships.json';

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
      },
      citizenships,
    };
  },
  methods: {
    onCitizenshipClicked(selectedCitizenship) {
      this.formData.nationality = selectedCitizenship.nationality;
    },
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
</style>
