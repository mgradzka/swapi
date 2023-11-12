<template>
  <teleport to="body">
    <div @click="$emit('close')"></div>
    <dialog open>
      <header>
        <slot name="header">
          {{ title }}
        </slot>
      </header>
      <section>
        <slot></slot>
      </section>

      <base-button @click="$emit('close')">Close</base-button>
    </dialog>
  </teleport>
</template>

<script>
import BaseButton from "./BaseButton.vue";
export default {
  components: {
    BaseButton,
  },
  props: ["title"],
  emits: ["close"],
};
</script>

<style scoped>
div {
  position: fixed;
  top: 0;
  left: 0;
  height: 100vh;
  width: 100%;
  background-color: rgba(0, 0, 0, 0.75);
  z-index: 10;
}

dialog {
  position: fixed;
  padding: 2rem;
  top: 20vh;
  left: 50%;
  transform: translateX(-50%);
  width: 30%;
  z-index: 100;
  border: none;
  background-color: black;
  border: 1px solid yellow;
  overflow: hidden;
  color: rgba(255, 255, 255, 0.8);
}

header,
section {
  margin-bottom: 2rem;
}
</style>
