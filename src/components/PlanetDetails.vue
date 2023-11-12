<template>
  <base-modal v-if="planetDetails"  @close="closePopup">
    <template #header>
      <h2>About the planet</h2>
    </template>
    <template #default>
      <p>Name: {{ planetDetails.name }}</p>
      <p>Diameter: {{ planetDetails.diameter }}</p>
      <p>Climate: {{ planetDetails.climate }}</p>
      <p>Population: {{ planetDetails.population }}</p>
      <!-- Add more details as needed -->
    </template>
  </base-modal>
</template>

<script>
// import { ref, onMounted } from 'vue';
import BaseModal from './BaseModal.vue';

export default {
  props: ['planetDetails'],
  emits: ['closePopup'],
  components: { BaseModal },
  setup(_, {emit}) {
    const closePopup = () => {
      // Emit the close event to notify the parent component
      // that the modal should be closed
      emit('closePopup');
    };

    return { closePopup };
  },
};
</script>
