<template>
  <div class="d-flex">
    <Sidebar :types="types" @selectType="selectType" @addType="showAddTypeForm" />
    <MainPage :selectedType="selectedType" :showAddForm="showAddForm" @fetchData="fetchData" />
  </div>
</template>

<script>
import axios from 'axios';
import Sidebar from './components/Sidebar.vue';
import MainPage from './components/MainPage.vue';

export default {
  components: {
    Sidebar,
    MainPage
  },
  data() {
    return {
      types: [],
      selectedType: null,
      showAddForm: false
    };
  },
  methods: {
    async fetchData() {
      try {
        const response = await axios.get('https://kominfo-backend-dev.vercel.app/v1/data');
        this.types = response.data.results;
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    },
    selectType(type) {
      this.selectedType = type;
      this.showAddForm = false;
    },
    showAddTypeForm() {
      this.selectedType = null;
      this.showAddForm = true;
    }
  },
  created() {
    this.fetchData();
  }
};
</script>
