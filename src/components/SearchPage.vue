<template>
  <div class="container my-4">
    <!-- Форма поиска -->
    <div class="row mb-3">
      <div class="col-12 col-md-4 d-flex mb-2 mb-md-0">
        <input
          type="text"
          v-model="search.origin"
          class="form-control"
          placeholder="Origin"
          id="origin"
          @keypress="allowLatinLetters"
        />
        <input
          type="text"
          disabled
          class="form-control ms-2 dho-input"
          placeholder="DH-O"
        />
      </div>
      <div class="col-12 col-md-4 text-center mb-2 mb-md-0">
        <svg xmlns="http://www.w3.org/2000/svg" width="40" height="25" fill="currentColor" class="bi bi-arrow-left-right glyphicon" viewBox="0 0 16 16">
          <path fill-rule="evenodd" d="M1 11.5a.5.5 0 0 0 .5.5h11.793l-3.147 3.146a.5.5 0 0 0 .708.708l4-4a.5.5 0 0 0 0-.708l-4-4a.5.5 0 0 0-.708.708L13.293 11H1.5a.5.5 0 0 0-.5.5m14-7a.5.5 0 0 1-.5.5H2.707l3.147 3.146a.5.5 0 1 1-.708.708l-4-4a.5.5 0 0 1 0-.708l4-4a.5.5 0 1 1 .708.708L2.707 4H14.5a.5.5 0 0 1 .5.5"/>
        </svg>
      </div>
      <div class="col-12 col-md-4 d-flex">
        <input
          type="text"
          v-model="search.destination"
          class="form-control"
          placeholder="Destination"
          id="destination"
          @keypress="allowLatinLetters"
        />
        <input
          type="text"
          disabled
          class="form-control ms-2 dho-input"
          placeholder="DH-D"
        />
      </div>
    </div>
    <div class="row mb-3 mt-3">
      <div class="col-12 col-md-2 mb-2 mb-md-0">
        <div class="d-flex">
          <div class="col-12">
            <select class="form-select w-100" @change="addTag($event)">
              <option disabled selected>Choose a tag</option>
              <option
                v-for="(tag, index) in availableTags"
                :key="index"
                :value="tag"
                :selected="isSelected(tag)"
              >
                {{ tag }}
              </option>
            </select>
          </div>
        </div>
        <div class="tag-container mt-2">
          <span
            v-for="(tag, index) in selectedTags"
            :key="index"
            class="tag-item"
          >
            {{ tag }}
            <button
              type="button"
              class="close-btn"
              aria-label="Close"
              @click="removeTag(index)"
            >×</button>
          </span>
        </div>
      </div>
      <div class="col-12 col-md-2 mb-2 mb-md-0">
        <select
          class="form-select w-100"
          v-model="selectedLoad"
          id="load"
        >
          <option>Full & Partial</option>
          <option>Full</option>
          <option>Partial</option>
        </select>
      </div>
      <div class="col-12 col-md-2 mb-2 mb-md-0">
        <input
          type="number"
          v-model="length"
          class="form-control w-100"
          placeholder="Length ft"
        />
      </div>
      <div class="col-12 col-md-2 mb-2 mb-md-0">
        <input
          type="number"
          v-model="weight"
          class="form-control w-100"
          placeholder="Weight ft"
        />
      </div>
      <div class="col-12 col-md-2 mb-2 mb-md-0">
        <div>
          <VueDatePicker v-model="dates" range format="MM/dd/yyyy" placeholder="Date Range*" class="w-100"></VueDatePicker>
        </div>
      </div>
      <div class="col-12 col-md-2">
        <button class="btn btn-primary w-100" @click="filterData">Search</button>
      </div>
    </div>

    <div>
      <div class="d-flex flex-column flex-md-row justify-content-between align-items-center text-center">
        <div class="d-flex flex-column flex-md-row justify-content-between align-items-center w-100 w-md-50 mb-3 mb-md-0">
      <span class="me-2">
        <svg xmlns="http://www.w3.org/2000/svg" fill="#000000" height="20px" width="20px" viewBox="0 0 489.645 489.645">
          <path d="M460.656,132.911c-58.7-122.1-212.2-166.5-331.8-104.1c-9.4,5.2-13.5,16.6-8.3,27c5.2,9.4,16.6,13.5,27,8.3c99.9-52,227.4-14.9,276.7,86.3c65.4,134.3-19,236.7-87.4,274.6c-93.1,51.7-211.2,17.4-267.6-70.7l69.3,14.5c10.4,2.1,21.8-4.2,23.9-15.6c2.1-10.4-4.2-21.8-15.6-23.9l-122.8-25c-20.6-2-25,16.6-23.9,22.9l15.6,123.8c1,10.4,9.4,17.7,19.8,17.7c12.8,0,20.8-12.5,19.8-23.9l-6-50.5c57.4,70.8,170.3,131.2,307.4,68.2C414.856,432.511,548.256,314.811,460.656,132.911z"/>
        </svg>
      </span>
          <div class="d-flex flex-column flex-column justify-content-between">
            <span>{{filteredData.length}} Results</span>
            <span>+2 Similar Results</span>
          </div>
          <div class="mt-2 mt-md-0">
            Sort by <span class="text-primary">{{activeSortKey}} - Newest</span>
          </div>
        </div>
        <div class="text-center w-100 w-md-auto">
          <span>$LANE RATE: $3,555 ($1.80/MI)</span>
        </div>
      </div>
    </div>

    <table class=" custom-table table table-striped">
      <tbody>
      <template v-for="(item, index) in filteredData" :key="index">
        <tr v-if="!index">
          <th @click="sortTable('age')" :class="{'activeSort': activeSortKey === 'age'}">
            <span>Age</span>
            <svg v-if="activeSortKey === 'age'" :class="{'rotate': sortOrder === -1}" width="18" height="18" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M12 19V5M5 12l7-7 7 7" stroke="#000" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </th>
          <th @click="sortTable('rate')" :class="{'activeSort': activeSortKey === 'rate'}">
            <span>Rate</span>
            <svg v-if="activeSortKey === 'rate'" :class="{'rotate': sortOrder === -1}" width="18" height="18" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M12 19V5M5 12l7-7 7 7" stroke="#000" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </th>
          <th @click="sortTable('tripFrom')" :class="{'activeSort': activeSortKey === 'tripFrom'}">
            <span>Trip</span>
            <svg v-if="activeSortKey === 'tripFrom'" :class="{'rotate': sortOrder === -1}" width="18" height="18" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M12 19V5M5 12l7-7 7 7" stroke="#000" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </th>
          <th>Pick Up</th>
          <th>Equipment</th>
          <th>Company</th>
        </tr>
        <tr style="cursor: pointer;" @click="item.active = !item.active">
          <td>
            <input type="checkbox" class="me-3">
            {{item.age}}
          </td>
          <td>{{ item.rate }}</td>
          <td>
            <div class="d-flex">
              <span class="me-4 text-primary">{{item.trip}}</span>
              <div class="timeline">
                <div class="timeline-item">
                  <div class="circle"></div>
                  <div class="content">
                    <span>{{item.tripFrom}}</span>
                  </div>
                </div>
                <div class="timeline-item">
                  <div class="circle"></div>
                  <div class="content">
                    <span>{{item.tripTo}}</span>
                  </div>
                </div>
              </div>
            </div>
          </td>
          <td>{{ item.pickUpFrom }} - {{ item.pickUpTo }}</td>
          <td>
            <span class="me-2">{{ item.equipment.charAt(0) }} </span>
            <span>{{ item.equipment }}</span>
          </td>
          <td>
            <div class="d-flex flex-column">
              <span class="text-primary">{{ item.company.name }}</span>
              <span class="text-primary">{{ item.company.email }}</span>
            </div>
          </td>
        </tr>
        <tr v-if="item.active">
          <td colspan="6">
            <InfoDetails :details="item" :dates="dates" />
          </td>
        </tr>
      </template>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import VueDatePicker from '@vuepic/vue-datepicker';
import InfoDetails from '@/components/InfoDetails.vue'

const selectedLoad = ref('Full & Partial');
const search = ref({
  origin: '',
  destination: '',
});
const length = ref();
const weight = ref();
const dates = ref([]);
const availableTags = ref(['Power Only', 'B-Train', 'Container'])
const selectedTags = ref([])
const addTag = (event) => {
  const tag = event.target.value;
  if (!selectedTags.value.includes(tag)) {
    selectedTags.value.push(tag);
  }
};

const removeTag = (index) => {
  selectedTags.value.splice(index, 1);
};

const isSelected = (tag) => {
  return selectedTags.value.includes(tag);
};

const data = ref([
  { age: '18m', rate: '$400', tripFrom: 'French Camp, CA', tripTo: 'Houston, TX', pickUpFrom: '6/23', pickUpTo: '7/2', equipment: 'Power Only',  load: 'Partial', weight: 300, length: 200, date: ['08/01/2024', '08/11/2024'], active: false, commodity: 'commodity1', referenceId: 1, comments: 'comments1', total: 400, trip: 1879, mile: 0.21, company: {name: 'Transloop Logistic Lic1', email: 'email1@example.com', address: 'Chicago,IL1', score: 97, daysToPay: 19, factorWith: 'OTR SOLUTIONS 1', insurance: 'PER LOAD INSURANCE ml 1'}},
  { age: '20m', rate: '$300', tripFrom: 'Red Bluff, CA', tripTo: 'Katy, TX', pickUpFrom: '6/24', pickUpTo: '7/2', equipment: 'B-Train', load: 'Full & Partial', weight: 100, length: 300, date: ['08/11/2024', '08/21/2024'], active: false, commodity: 'commodity2', referenceId: 2, comments: 'comments2', total: 500, trip: 1878, mile: 0.22, company: {name: 'Transloop Logistic Lic2', email: 'email2@example.com', address: 'Chicago,IL2' , score: 96, daysToPay: 20, factorWith: 'OTR SOLUTIONS 2', insurance: 'PER LOAD INSURANCE ml 2'}},
  { age: '22m', rate: '$350', tripFrom: 'New York, NY', tripTo: 'Miami, FL', pickUpFrom: '6/25', pickUpTo: '7/2', equipment: 'Container', load: 'Partial', weight: 100, length: 200, date: ['08/22/2024', '08/26/2024'], active: false, commodity: 'commodity3', referenceId: 3, comments: 'comments3', total: 600, trip: 1877, mile: 0.23, company: {name: 'Transloop Logistic Lic3', email: 'email3@example.com', address: 'Chicago,IL3' , score: 95, daysToPay: 21, factorWith: 'OTR SOLUTIONS 3', insurance: 'PER LOAD INSURANCE ml 3'}},
  { age: '12m', rate: '$500', tripFrom: 'Los Angeles, CA', tripTo: 'San Francisco, CA', pickUpFrom: '6/26', pickUpTo: '7/2', equipment: 'Power Only', load: 'Full', weight: 100, length: 300, date: ['08/26/2024', '08/28/2024'], active: false, commodity: 'commodity4', referenceId: 4, comments: 'comments4', total: 700, trip: 1876, mile: 0.24, company: {name: 'Transloop Logistic Lic4', email: 'email4@example.com', address: 'Chicago,IL4' , score: 94, daysToPay: 22, factorWith: 'OTR SOLUTIONS 4', insurance: 'PER LOAD INSURANCE ml 4'}},
  { age: '15m', rate: '$450', tripFrom: 'Chicago, IL', tripTo: 'Dallas, TX', pickUpFrom: '6/27', pickUpTo: '7/2', equipment: 'B-Train', load: 'Full', weight: 200, length: 200, date: ['08/06/2024', '08/20/2024'], active: false, commodity: 'commodity5', referenceId: 5, comments: 'comments5', total: 400, trip: 1875, mile: 0.25, company: {name: 'Transloop Logistic Lic5', email: 'email5@example.com', address: 'Chicago,IL' , score: 93, daysToPay: 23, factorWith: 'OTR SOLUTIONS 5', insurance: 'PER LOAD INSURANCE ml 5'}},
]);

const activeSortKey = ref('age');  // Use a key to track the currently sorted column
const sortKey = ref('age');
const sortOrder = ref(1);
const filteredData = ref([...data.value]);
function formatDate(date) {
  const day = date.getDate().toString().padStart(2, '0');
  const month = (date.getMonth() + 1).toString().padStart(2, '0');
  const year = date.getFullYear();

  return `${month}/${day}/${year}`;
}


const filterData = () => {
  filteredData.value = data.value.filter(item => {
    const hasValidDates = Array.isArray(dates.value) && dates.value.length === 2;
    const startDate = hasValidDates ? formatDate(dates.value[0]) : null;
    const endDate = hasValidDates ? formatDate(dates.value[1]) : null;

    const matchesOrigin = item.tripFrom.toLowerCase().includes(search.value.origin.toLowerCase());
    const matchesDestination = item.tripTo.toLowerCase().includes(search.value.destination.toLowerCase());
    const matchesWeight = weight.value === undefined || weight.value === '' || item.weight === weight.value
    const matchesLength = length.value === undefined || length.value === '' || item.length === length.value
    const matchesLoad = selectedLoad.value === item.load;
    const matchesDate = !hasValidDates || (startDate <= item.date[1] && endDate >= item.date[0]);
    const matchesEquipment = selectedTags.value.length === 0 || selectedTags.value.includes(item.equipment);
    return matchesOrigin && matchesDestination && matchesEquipment && matchesLoad && matchesWeight && matchesLength && matchesDate;
  });

  if (sortKey.value) {
    filteredData.value.sort((a, b) => (a[sortKey.value] > b[sortKey.value] ? sortOrder.value : -sortOrder.value));
  }
};

const sortTable = (key) => {
  if (sortKey.value === key) {
    sortOrder.value = -sortOrder.value;
  } else {
    sortOrder.value = 1;
    sortKey.value = key;
  }
  activeSortKey.value = key;
  filterData();
};

const allowLatinLetters = (event) => {
  const key = event.key;
  if (!/^[a-zA-Z\s]*$/.test(key)) {
    event.preventDefault();
  }
};

const updateSearchField = (autocomplete, field) => {
  const place = autocomplete.getPlace();
  if (place && place.address_components) {
    const cityComponent = place.address_components.find(component => component.types.includes('locality'));
    if (cityComponent) {
      search.value[field] = cityComponent.long_name;
    }
  }
};

onMounted(() => {
  const originInput = document.getElementById('origin');
  const destinationInput = document.getElementById('destination');

  const autocompleteOptions = {
    types: ['(cities)'],
    componentRestrictions: { country: 'us' },
  };

  const originAutocomplete = new google.maps.places.Autocomplete(originInput, autocompleteOptions);
  const destinationAutocomplete = new google.maps.places.Autocomplete(destinationInput, autocompleteOptions);

  originAutocomplete.addListener('place_changed', () => updateSearchField(originAutocomplete, 'origin'));
  destinationAutocomplete.addListener('place_changed', () => updateSearchField(destinationAutocomplete, 'destination'));
  filterData()
});

</script>
<style>
.glyphicon {
  display: inline-block;
  font-size: 1.5rem;
  fill: blue;
}
.tag-container {
  display: flex;
  flex-wrap: wrap;
}

.tag-item {
  display: flex;
  align-items: center;
  background-color: #e0e0e0;
  color: #333;
  border-radius: 15px;
  padding: 5px 10px;
  margin: 5px;
  font-size: 0.875rem;
  position: relative;
}

.close-btn {
  background: none;
  border: none;
  color: #555;
  font-size: 1rem;
  margin-left: 8px;
  cursor: pointer;
}

.close-btn:hover {
  color: #000;
}

.dho-input {
  width: 80px;
}

td:not(:last-child) {
  position: relative;
}

td:not(:last-child)::after {
  content: '';
  position: absolute;
  top: 10%;
  bottom: 10%;
  right: 0;
  width: 1px;
  background-color: lightgray;
}

.activeSort {
  color: blue !important;
}
.activeSort svg {
  display: inline-block;
}
.rotate {
  transform: rotate(180deg);
}
.timeline {
  display: flex;
  flex-direction: column;
  position: relative;
}
.timeline::before {
  content: '';
  position: absolute;
  left: 3px;
  top: 7px;
  height: 68%;
  width: 2px;
  background: repeating-linear-gradient(
    to bottom,
    #ccc,
    #ccc 4px,
    transparent 4px,
    transparent 8px
  );
}
.timeline-item {
  display: flex;
  align-items: center;
  position: relative;
}
.circle {
  width: 8px;
  height: 8px;
  background-color: blue;
  border-radius: 50%;
  margin-right: 10px;
  position: relative;
  z-index: 1;
}
.content {
  display: flex;
  flex-direction: column;
}
.custom-table,
.custom-table > thead,
.custom-table > tbody,
.custom-table tr
{
  width: 100%;
}
.custom-table > tbody{
  display: table;
}
.custom-table tr{
  display: table-row;
}
.custom-table tr td, .custom-table tr th{
  display: table-cell;
}
@media (max-width: 768px) {
  .dho-input {
    width: 60px;
  }

  .form-select,
  .form-control {
    font-size: 0.875rem;
    padding: 0.375rem 0.75rem;
  }

  .glyphicon {
    width: 30px;
    height: 20px;
  }
  .table {
    font-size: 0.875rem;
  }
}

@media (max-width: 1300px) {
  .table {
    display: inline-block;
    overflow-x: auto;
    white-space: nowrap;
  }
}
@media (max-width: 990px) {
  .table {
  }
}
</style>