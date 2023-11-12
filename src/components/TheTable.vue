<template>
  <div class="button-container" v-if="isData">
    <input v-model="searchQuery" type="text" placeholder="Search the galaxy!" />
    <base-button @click="resetQuery">Reset</base-button>
  </div>
  <table>
    <thead v-if="isData">
      <tr>
        <th @click="sortTable('name')"><span>Name</span></th>
        <th @click="sortTable('height')"><span>Height</span></th>
        <th @click="sortTable('mass')"><span>Mass</span></th>
        <th @click="sortTable('created')"><span>Created</span></th>
        <th @click="sortTable('edited')"><span>Edited</span></th>
        <th @click="sortTable('homeworldName')"><span>Planet</span></th>
      </tr>
    </thead>
    <tbody>
      <table-row
        v-for="person in sortedPeopleData"
        :key="person.name"
        :person="person"
      ></table-row>
    </tbody>
  </table>
  <the-spinner v-if="isLoading" />
</template>

<script>
import { ref, computed, onMounted } from "vue";
import TableRow from "./TableRow.vue";
import BaseButton from "./BaseButton.vue";
import TheSpinner from "./TheSpinner.vue";

export default {
  name: "TheTable",
  components: {
    TableRow,
    BaseButton,
    TheSpinner,
  },
  setup() {
    const isData = ref(false);
    const peopleData = ref([]);
    const isLoading = ref(false);
    const sortKey = ref("");
    const sortDirection = ref(1);
    const searchQuery = ref("");

    const sortedPeopleData = computed(() => {
      const key = sortKey.value;
      const direction = sortDirection.value;

      return peopleData.value
        .filter((person) => {
          const query = searchQuery.value.toLowerCase();
          return person.name.toLowerCase().includes(query);
        })
        .slice()
        .sort((a, b) => {
          const valueA = getColumnValue(a, key);
          const valueB = getColumnValue(b, key);

          return valueA > valueB ? direction : -direction;
        });
    });

    const getColumnValue = (item, key) => {
      if (key === "homeworldName") {
        return item.homeworldName ? item.homeworldName.toLowerCase() : "";
      } else if (["name", "created", "edited"].includes(key)) {
        return item[key].toLowerCase();
      } else if (["height", "mass"].includes(key)) {
        return parseFloat(item[key]) || 0;
      } else {
        return null; 
      }
    };

    const sortTable = (key) => {
      if (sortKey.value === key) {
        sortDirection.value = -sortDirection.value; 
      } else {
        sortKey.value = key;
        sortDirection.value = 1; 
      }
    };

    const resetQuery = () => {
      searchQuery.value = "";
    };

    const getPeople = async () => {
      try {
        isLoading.value = true;
        const response = await fetch("https://swapi.dev/api/people/");
        if (!response.ok) {
          throw new Error("Failed to fetch people data");
        }
        const data = await response.json();

        isLoading.value = false;
        isData.value = true;
        console.log(data.results);
        peopleData.value = data.results;

       
        await fetchPlanetNames();
      } catch (error) {
        console.error("Error fetching people data:", error);
      } finally {
        isLoading.value = false;
      }
    };

    const fetchPlanetNames = async () => {
      try {
        await Promise.all(
          peopleData.value.map(async (person) => {
            const response = await fetch(person.homeworld);
            if (!response.ok) {
              throw new Error(
                `Failed to fetch homeworld data for ${person.name}`
              );
            }
            const data = await response.json();
            
            person.homeworldName = data.name;
          })
        );
      } catch (error) {
        console.error("Error fetching homeworld data:", error);
      }
    };

    onMounted(() => {
      getPeople();
    });

    return {
      isData,
      peopleData,
      isLoading,
      sortKey,
      sortDirection,
      sortedPeopleData,
      sortTable,
      searchQuery,
      resetQuery,
    };
  },
};
</script>
