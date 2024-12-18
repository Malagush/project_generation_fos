<template>
  <div>
    <h1>{{ formData.title }}</h1>
    <form @submit.prevent="handleSubmit"> 
      <div v-for="control in formData.controls" :key="control.id">
        <label v-if="control.control === 'LABEL'" :for="control.id" class="form-label">
          {{ control.caption }}
        </label>
        <input
        v-else-if="control.control === 'TEXT'" 
          :id="control.id"
          :placeholder="control.caption"
          v-model="formValues[control.id]"
          :required="control.required === 'true'"
          class="form-control"
          :tabindex="control.tabIndex"
          @input="updateValue(control.id, $event.target.value)"
        />
        <select
        v-else-if="control.control === 'SELECT'"
          :id="control.id"
          v-model="formValues[control.id]"
          :required="control.required === 'true'"
          class="form-control"
          :tabindex="control.tabIndex"
          @change="updateValue(control.id, $event.target.value)"
        >
          <option disabled value="">Выберите вариант</option>
          <option v-for="option in control.options" :key="option" :value="option">{{ option }}</option>
        </select>
        <div v-else-if="control.control === 'CHECKBOX'" class="form-check"> 
          <input
            type="checkbox"
            :id="control.id"
            v-model="formValues[control.id]"
            :required="control.required === 'true'"
            @change="updateValue(control.id, $event.target.checked)" 
            class="form-check-input"
          />
          <label :for="control.id" class="form-check-label">
            {{ control.caption }}
          </label>
        </div>
        <div v-else-if="errorMessages.length && control.required === 'true' && !formValues[control.id]" class="error-message">
          Поле "{{ control.caption }}" обязательно для заполнения. 
        </div>
        <button
        v-else-if="control.control === 'BUTTON'" 
          type="button"
          @click="handleButtonClick(control.caption)"
          class="form-control"
        >
          {{ control.caption }}
        </button>
      </div>
      <div v-if="successMessage" class="success-message">{{ successMessage }}</div> 
    </form>
  </div>
</template>

<script>
export default {
  props: {
    formData: Object, // Данные формы / настройки / разметка
    formValues: Object, // Значения полей формы
    errorMessages: Array, // Сообщения об ошибках
    successMessage: String, // Сообщение об успешной отправке формы
    handleSubmit: Function, // Функция обработки отправки формы
    handleButtonClick: Function, // Функция обработки нажатия на кнопку
  },
  methods: {
    updateValue(id, value) {
      this.$emit('updateFormValues', id, value); // Эмитация действия, для обновления значений
    },
  },
};
</script>