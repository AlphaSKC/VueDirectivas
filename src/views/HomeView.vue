<script setup lang="ts">
  import { ref, onMounted } from 'vue';
  import type { Ref } from 'vue'; 

  const name: Ref<string> = ref('');
  const lastname: Ref<string> = ref('');
  const age: Ref<number> = ref(0);
  const gender: Ref<string> = ref('');
  const errors: Ref<Array<{ field: string; message: string }>> = ref([]);

  const validateForm = () => {
    clearErrors();

    if (name.value.length < 5 || name.value.length > 18) {
      errors.value.push({ field: 'name', message: 'El nombre debe tener entre 5 y 18 caracteres' });
    }

    if (lastname.value === name.value) {
      errors.value.push({ field: 'lastname', message: 'El apellido no puede ser igual al nombre' });
    }

    if(age.value < 0 || age.value > 60){
      errors.value.push({ field: 'age', message: 'La edad debe estar entre 0 y 60 años' });
    }
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
    <div>
      <input @input="validateForm()" v-model="name" type="text" placeholder="Nombre">
    </div>
    <span v-if="errors.some(error => error.field === 'name')" class="error">
      {{ errors.find(error => error.field === 'name')?.message }}
    </span>
    <div>
      <input @input="validateForm()" v-model="lastname" type="text" placeholder="Apellido">
    </div>
    <span v-if="errors.some(error => error.field === 'lastname')" class="error">
      {{ errors.find(error => error.field === 'lastname')?.message }}
    </span>
    <div>
      <input @input="validateForm()" v-model="age" type="number" placeholder="Edad">
    </div>
    <span v-if="errors.some(error => error.field === 'age')" class="error">
      {{ errors.find(error => error.field === 'age')?.message }}
    </span>
    <div>
      <label for="gender">Género:</label>
      <select v-model="gender" @change="validateForm()">
        <option value="masculino">Masculino</option>
        <option value="femenino">Femenino</option>
        <option value="otro">Otro</option>
      </select>
      <input v-if="gender === 'otro'" type="text" placeholder="Especifica otro género">
    </div>
  </main>
</template>

<style scoped>
  .error {
    color: white;
    background-color: red;
  }
</style>