<template>
  <tr>
    <td>{{ person.name }}</td>
    <td>{{ person.height }} cm</td>
    <td>{{ person.mass }} kg</td>
    <td>{{ formatDate(person.created) }}</td>
    <td>{{ formatDate(person.edited) }}</td>
    <td @click="openPopup"><span class="planet">{{ homeworldName }}</span></td>
  </tr>

  <planet-details
    v-if="popupVisible"
    @closePopup="closePopup"
    :planetDetails="planetDetails"
  />
</template>

<script>
import { ref, onMounted } from 'vue';
import PlanetDetails from './PlanetDetails.vue';

export default {
  name: 'TableRow',
  components: { PlanetDetails },
  props: ['person'],
  setup(props) {
    const homeworldName = ref('');
    const planetDetails = ref(null);
    const popupVisible = ref(false);

    const fetchData = async (url) => {
      try {
        const response = await fetch(url);
        if (response.ok) {
          return await response.json();
        }
      } catch (error) {
        console.error('Error fetching data:', error);
      }
      return null;
    };

    const getHomeworldName = async () => {
      const homeworldData = await fetchData(props.person.homeworld);
      if (homeworldData) {
        homeworldName.value = homeworldData.name;
      }
    };

    const getPlanetDetails = async () => {
      planetDetails.value = await fetchData(props.person.homeworld);
    };

    const formatDate = (date) => {
      return new Date(date).toLocaleDateString();
    };

    const openPopup = () => {
      popupVisible.value = true;
      getPlanetDetails();
    };

    const closePopup = () => {
      popupVisible.value = false;
      planetDetails.value = null;
    };

    onMounted(() => {
      getHomeworldName();
    });

    return { homeworldName, planetDetails, popupVisible, formatDate, openPopup, closePopup };
  },
};
</script>

<style scoped>
.planet {
  cursor: pointer;
}
</style>
