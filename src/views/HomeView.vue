<script setup lang="ts">
import { ref, onMounted } from 'vue';
import type { Ref } from 'vue';

const name: Ref<string> = ref('');
const lastname: Ref<string> = ref('');
const age: Ref<number> = ref(0);
const gender: Ref<string> = ref('');
const other: Ref<string> = ref('');
const errors: Ref<Array<{ field: string; message: string }>> = ref([]);
const isValid: Ref<boolean> = ref(false);

const validateForm = () => {
  clearErrors();

  if (name.value.length < 5 || name.value.length > 18) {
    errors.value.push({ field: 'name', message: 'El nombre debe tener entre 5 y 18 caracteres' });
  }

  if (lastname.value === name.value) {
    errors.value.push({ field: 'lastname', message: 'El apellido no puede ser igual al nombre' });
  }

  if (age.value < 0 || age.value > 60) {
    errors.value.push({ field: 'age', message: 'La edad debe estar entre 0 y 60 años' });
  }

  if (gender.value === 'otro' && !other.value.trim()) {
    errors.value.push({ field: 'other', message: 'No puede quedar vacio el campo' });
  }

  isValid.value = errors.value.length === 0;
};

const clearErrors = (field?: string) => {
  errors.value = field ? errors.value.filter(error => error.field !== field) : [];
};

onMounted(() => {
  validateForm();
});
</script>

<template>
  <main>
    <form class="form">
      <div class="container">
        <label>Nombre:</label>
        <input @input="validateForm()" v-model="name" type="text" placeholder="Ingresa tu nombre">
        <span v-if="errors.some(error => error.field === 'name')" class="error">
          {{ errors.find(error => error.field === 'name')?.message }}
        </span>
      </div>
      <div class="container">
        <label>Apellido:</label>
        <input @input="validateForm()" v-model="lastname" type="text" placeholder="Ingresa tu apellido">
        <span v-if="errors.some(error => error.field === 'lastname')" class="error">
          {{ errors.find(error => error.field === 'lastname')?.message }}
        </span>
      </div>
      <div class="container">
        <label>Edad:</label>
        <input @input="validateForm()" v-model="age" type="number">
        <span v-if="errors.some(error => error.field === 'age')" class="error">
          {{ errors.find(error => error.field === 'age')?.message }}
        </span>
      </div>
      <div class="container">
        <label>Género:</label>
        <select v-model="gender" @change="validateForm()">
          <option value="masculino">Masculino</option>
          <option value="femenino">Femenino</option>
          <option value="otro">Otro</option>
        </select>
        <div class="container" v-if="gender === 'otro'">
        <label>Otro:</label>
        <input @input="validateForm()" v-model="other" type="text" placeholder="Especifica otro género">
        <span v-if="errors.some(error => error.field === 'other')" class="error">
          {{ errors.find(error => error.field === 'other')?.message }}
        </span>
      </div>
      </div>
      <div class="container">
        <button type="submit" :disabled="!isValid">Enviar</button>
      </div>
    </form>
  </main>
</template>

<style scoped>
.form {
  width: 30vw;
  margin: 0 auto;
  padding: 20px;
  background-color: #34495E;
  border-radius: 8px;
  border: 2px solid #41B883;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.container {
  margin: 10px 0 10px 0;
}

.container button{
  margin-left: auto;
  margin-right: auto;
  display: block;
}

label {
  display: block;
  font-size: 16px;
  font-weight: bold;
  color: #41B883;
}

input, select {
  width: 100%;
  padding: 10px;
  margin-top: 5px;
  box-sizing: border-box;
  font-size: 16px; 
  border: 2px solid black; 
  border-radius: 5px;
}

button {
  background-color: #41B883;
  color: white;
  padding: 10px;
  font-size: 16px; 
  font-weight: bold;
  border: 2px solid black;
  border-radius: 5px;
  cursor: pointer;
}

button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

.error {
  color: white;
  background-color: #3755da;
  padding: 5px;
  border: 2px solid black;
  border-radius: 5px;
  display: block;
  margin-top: 5px;
}
</style>