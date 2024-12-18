<template>
  <div id="app">
    <DynamicForm />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import DynamicForm from './components/DynamicForm.vue';
import './App.css';

@Component({
  components: {
    DynamicForm,
  },
})
export default class App extends Vue {}
</script>
