<template>
  <div class="container my-4">
    <!-- Форма поиска выше -->
    <table class="table table-bordered">
      <thead>
      <tr>
        <th @click="sortTable('age')">Age</th>
        <th @click="sortTable('rate')">Rate</th>
        <th @click="sortTable('trip')">Trip</th>
        <th>Pick Up</th>
        <th>Equipment</th>
        <th>Company</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="item in filteredData" :key="item.trip">
        <td>{{ item.age }}</td>
        <td>{{ item.rate }}</td>
        <td>{{ item.trip }}</td>
        <td>{{ item.pickUp }}</td>
        <td>{{ item.equipment }}</td>
        <td>{{ item.company }}</td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const data = ref([
  { age: '18m', rate: '$400', trip: 'French Camp, CA to Houston, TX', pickUp: '6/23', equipment: 'Power Only', company: 'Transloop Logistic' },
]);

const sortKey = ref('');
const sortOrder = ref(1); // 1 - по возрастанию, -1 - по убыванию

const sortTable = (key) => {
  sortOrder.value = sortKey.value === key ? -sortOrder.value : 1;
  sortKey.value = key;
};

const filteredData = computed(() => {
  let sortedData = [...data.value];
  if (sortKey.value) {
    sortedData.sort((a, b) => (a[sortKey.value] > b[sortKey.value] ? sortOrder.value : -sortOrder.value));
  }
  return sortedData;
});
</script>
