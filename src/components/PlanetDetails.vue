<template>
  <base-modal @close="closePopup">
    <template #header v-if="planetDetails">
      <h2>About the planet</h2>
    </template>
    <template #default v-if="planetDetails">
      <p>Name: {{ planetDetails.name }}</p>
      <p>Diameter: {{ planetDetails.diameter }}</p>
      <p>Climate: {{ planetDetails.climate }}</p>
      <p>Population: {{ planetDetails.population }}</p>
    </template>
    <the-spinner v-if="!planetDetails" />
  </base-modal>
</template>

<script>
import BaseModal from "./BaseModal.vue";
import TheSpinner from "./TheSpinner.vue";

export default {
  props: ["planetDetails"],
  emits: ["closePopup"],
  components: { BaseModal, TheSpinner },
  setup(_, { emit }) {
    const closePopup = () => {
      emit("closePopup");
    };

    return { closePopup };
  },
};
</script>
