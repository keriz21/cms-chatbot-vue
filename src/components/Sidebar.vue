<template>
  <div class="bg-light border-end vh-100" style="width: 250px;">
    <div class="p-3">
      <h3>CMS CHATBOT</h3>
      <button class="btn btn-primary w-100 mb-3" @click="addType">Add Type</button>
      <ul class="list-group">
        <li
          v-for="type in types"
          :key="type.id"
          class="list-group-item list-group-item-action"
          @click="selectType(type)"
        >
          {{ type.type }}
        </li>
      </ul>
    </div>

    <div class="p-3">
      <button class="btn btn-danger w-100 mb-3">Train Ulang</button>
    </div>
  </div>
</template>

<script>
export default {
  props: ['types'],
  methods: {
    selectType(type) {
      this.$emit('selectType', type);
    },
    addType() {
      this.$emit('addType');
    }
  }
};
</script>
