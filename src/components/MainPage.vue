<template>
  <div class="flex-grow-1 p-4 d-flex flex-column" style="height: 100vh; overflow-y: auto;">
    <div v-if="showAddForm">
      <h2>Menambah Type</h2>
      <form @submit.prevent="submitNewType">
        <div class="mb-3">
          <label for="newType" class="form-label">Type Baru</label>
          <input v-model="newType" type="text" class="form-control" id="newType" placeholder="Masukkan nama type baru">
        </div>
        <button type="submit" class="btn btn-success">Submit</button>
      </form>
    </div>

    <div v-else-if="selectedType" class="flex-grow-1">
      <h2>{{ selectedType.type }}</h2>
      <ul class="list-group mb-4">
        <li v-for="step in selectedType.content" :key="step._id" class="list-group-item">
          <strong>{{ step.step }}:</strong> {{ step.description }}
        </li>
      </ul>

      <!-- Form untuk menambah step baru -->
      <div>
        <h5>Tambah Step Baru</h5>
        <form @submit.prevent="submitNewStep">
          <input v-model="newStepDescription" type="text" class="form-control mb-2" placeholder="Deskripsi step baru">
          <button type="submit" class="btn btn-primary">Add Step</button>
        </form>
      </div>
    </div>

    <div v-else>
      <h2>Silakan pilih atau tambahkan type baru dari sidebar.</h2>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  props: ['selectedType', 'showAddForm'],
  data() {
    return {
      newStepDescription: '',
      newType: ''
    };
  },
  methods: {
    async submitNewStep() {
      if (!this.newStepDescription.trim()) {
        alert('Deskripsi step tidak boleh kosong');
        return;
      }

      try {
        const stepNumber = this.selectedType.content.length + 1; // Menentukan nomor step berikutnya
        const response = await axios.post(`https://kominfo-backend-dev.vercel.app/v1/data/${this.selectedType.id}/content`, {
          step: stepNumber,
          description: this.newStepDescription
        });

        // Update selectedType dengan step baru
        this.selectedType.content.push(response.data);
        this.newStepDescription = ''; // Reset form setelah submit
      } catch (error) {
        console.error('Error menambahkan step:', error);
      }
    },

    async submitNewType() {
      if (!this.newType.trim()) {
        alert('Nama type tidak boleh kosong');
        return;
      }

      try {
        await axios.post('https://kominfo-backend-dev.vercel.app/v1/data', {
          type: this.newType
        });
        this.newType = ''; // Reset form setelah submit
        this.$emit('fetchData'); // Mengambil data ulang setelah menambahkan type baru
      } catch (error) {
        console.error('Error menambahkan type:', error);
      }
    }
  }
};
</script>
