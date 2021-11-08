<template>
  <div class="form">
    <form action="">
      <h2>Личные данные</h2>
      <div class="row">
        <label for="lastName" class="block">Фамилия</label>
        <input id="lastName" />
      </div>

      <div class="row">
        <label for="firstName" class="block">Имя</label>
        <input id="lastName" />
      </div>

      <div class="row">
        <label for="middleName" class="block">Отчество</label>
        <input id="middleName" />
      </div>

      <div class="row">
        <label for="birthDate" class="block">Дата рождения</label>
        <input id="birthDate" placeholder="дд.мм.гггг" />
      </div>

      <div class="row">
        <label for="email" class="block">E-mail</label>
        <input id="email" />
      </div>

      <div class="row">
        <label class="block">Пол</label>
        <input type="radio" id="maleGender" name="gender" checked />
        <label for="maleGender">Мужской</label>
        <input type="radio" id="femaleGender" name="gender" />
        <label for="maleGender">Женский</label>
      </div>

      <button>Отправить</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  methods: {},
};
</script>

<style scoped>
.row {
  margin: 15px 0;
}
.block {
  display: block;
}
</style>
