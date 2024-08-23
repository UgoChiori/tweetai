<template>
  <div class=" align-center">
    <h1 class="text-3xl font-bold text-center">Autobot Count</h1>
    <p class="text-3xl font-normal text-center">{{ count }}</p>
  </div>
</template>

<script lang="ts">
import { ref, onMounted } from 'vue';
import axios from 'axios';

export default {
  name: 'AutobotCount',
  setup() {
    const count = ref(0);

    const fetchAutobotCount = async () => {
      try {
        const response = await axios.get('http://localhost:2000/autobot-count');
        count.value = response.data.count;
      } catch (error) {
        console.error('Error fetching Autobot count:', error);
      }
    };

    onMounted(() => {
      fetchAutobotCount();
      setInterval(fetchAutobotCount, 30000);
    });

    return {
      count,
    };
  },
};
</script>

<style scoped>
/* Add styles here if needed */
</style>
