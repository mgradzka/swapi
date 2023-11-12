<template>
  <main>
    <h1>Star Wars api</h1>

    <the-table> </the-table>
  </main>
</template>

<script>
import TheTable from "./components/TheTable.vue";

export default {
  name: "App",
  components: {
    TheTable,
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Montserrat&display=swap");

@font-face {
  font-family: "Soloist";
  src: url("./assets/Soloist-Z6M8.otf") format("opentype");
  font-weight: normal;
  font-style: normal;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  scroll-behavior: smooth;
}

body {
  font-family: "Montserrat", sans-serif;
  background-image: url("./assets/sky.jpg");
  background-size: cover;
  height: 40vw;
  background-position-y: 45%;
  width: 100%;
  height: 100vh;
}

main {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  height: 100vh;
}

h1,
h2 {
  font-family: "Soloist", sans-serif;
  color: yellow;
}

h1 {
  font-size: 2.5rem;
  margin-bottom: 2rem;
}

h2 {
  font-size: 1.8rem;
}

table {
  border-collapse: collapse;
  font-size: 0.9em;
  min-width: 700px;
  overflow: hidden;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.15);
}

table thead tr {
  background-color: rgb(220, 220, 23);
  color: black;
  text-align: left;
  font-weight: bold;
}

td,
th {
  padding: 1rem;
}

th span {
  cursor: pointer;
  border-bottom: 1px solid black;
}

table tbody tr:nth-of-type(even) {
  background-color: black;
  color: rgba(255, 255, 255, 0.8);
}

table tbody tr:nth-of-type(odd) {
  background-color: rgb(12,12,12);
  color: rgba(255, 255, 255, 0.8);
}

table tbody tr:last-of-type {
  border-bottom: 1px solid yellow;
}
input {
  padding: 0.5rem;
  border: none;
  outline: none;
  font-family: "Montserrat", sans-serif;
}

.button-container {
  margin-bottom: 2rem;
}
</style>
