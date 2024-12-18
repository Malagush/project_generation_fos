<template>
  <DynamicFormTemplate
    :formData="formData"
    :formValues="formValues"
    :errorMessages="errorMessages"
    :successMessage="successMessage"
    :handleSubmit="handleSubmit"
    :handleButtonClick="handleButtonClick"
    @updateFormValues="updateFormValues"
  />
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import formData from '@/assets/formData.json'; 
import DynamicFormTemplate from './DynamicFormTemplate.vue'; 
import './DynamicForm.css';

@Component({
  components: {
    DynamicFormTemplate,
  },
})
export default class DynamicForm extends Vue {
  // Данные формы загружаются пока что из захардкоженного JSON файла
  formData = formData;
  // Объект с значениями полей формы
  formValues: { [key: string]: string | boolean } = {};
  // Текста выводимых сообщений
  errorMessages: string[] = [];
  successMessage: string | null = null;

  created() {
    this.loadFormData();
  }

  handleButtonClick(caption: string) {
    // Обработка кнопки 
    const button = this.formData.controls.find(control => control.caption === caption && control.control === 'BUTTON');
    if (button) {
      const actionType = button.actionType;
      if (actionType === 'submit') {
        this.handleSubmit(); 
      } else if (actionType === 'reset') {
        this.handleCancel();
      }
    }
  }

  updateFormValues(id: string, value: string | boolean) {
    // Обновление значения(1/n) формы
    this.$set(this.formValues, id, value);
    sessionStorage.setItem('formData', JSON.stringify(this.formValues)); 
  }

  handleSubmit() {
    this.errorMessages = [];
    this.successMessage = null; 
    // Проверка обязательных полей на заполнение
    for (const control of this.formData.controls) {
      if (control.required === "true" && !this.formValues[control.id]) {
        this.errorMessages.push(`Поле "${control.caption}" обязательно для заполнения.`);
      }
    }
    if (this.errorMessages.length === 0) {
      console.log("Форма успешно отправлена. Данные представлены ниже: ", this.formValues);
      sessionStorage.setItem('formData', JSON.stringify(this.formValues));
      this.successMessage = "Форма успешно отправлена!";
    }
  }

  handleCancel() {
    // Сброс, полный
    this.formValues = {};
    sessionStorage.removeItem('formData');
    this.successMessage = null; 
  }

  loadFormData() {
    // Загрузка формы на старте, если данные были сохранены в хранилище сессии
    const savedData = sessionStorage.getItem('formData');
    if (savedData) {
      this.formValues = JSON.parse(savedData);
    }
  }
}
</script>