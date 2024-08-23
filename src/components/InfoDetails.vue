<template>
  <div class="w-100 d-flex justify-content-between">
    <div class="trip w-100">
      <div class="details-title">
        <span>TRIP</span>
        <span>{{details.trip}} mi</span>
      </div>
      <div class="section-content">
        <div class="d-flex justify-content-between p-2">
          <div class="timeline">
            <div class="timeline-item">
              <div class="circle"></div>
              <div class="content">
                <p>{{details.tripFrom}}</p>
                <p class="date">{{details.date[0]}} - {{details.date[1]}}</p>
              </div>
            </div>
            <div class="timeline-item">
              <div class="circle"></div>
              <div class="content">
                <p>{{details.tripTo}}</p>
              </div>
            </div>
          </div>
          <div>
            <svg width="30" height="30" viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
              <g fill="none" stroke="#000000" stroke-width="10" stroke-linecap="round">
                <line x1="30" y1="70" x2="70" y2="30" />
                <line x1="70" y1="30" x2="30" y2="30" />
                <line x1="70" y1="30" x2="70" y2="70" />
              </g>
            </svg>
          </div>
        </div>
        <div>
          <p class="fw-bold mb-2">EQUIPMENT</p>
          <div class="w-75 d-flex justify-content-between mb-1">
            <span>Load</span>
            <span class="fw-bold">{{details.load}}</span>
          </div>
          <div class="w-75 d-flex justify-content-between mb-1">
            <span>Truck</span>
            <span class="fw-bold">{{details.equipment}}</span>
          </div>
          <div class="w-75 d-flex justify-content-between mb-1">
            <span>Length</span>
            <span class="fw-bold">{{details.length}}</span>
          </div>
          <div class="w-75 d-flex justify-content-between mb-1">
            <span>Weight</span>
            <span class="fw-bold">{{details.weight}}</span>
          </div>
          <div class="w-75 d-flex justify-content-between mb-1">
            <span>Commodity</span>
            <span class="fw-bold">{{details.commodity}}</span>
          </div>
          <div class="w-75 d-flex justify-content-between mb-1">
            <span>Reference ID</span>
            <span class="fw-bold">{{details.referenceId}}</span>
          </div>
        </div>
        <div class="mt-4">
          <p class="fw-bold mb-2">COMMENT</p>
          <span>{{details.comments}}</span>
        </div>
      </div>
    </div>
    <div class="rate w-100">
      <div class="details-title">
        <span>RATE</span>
      </div>
      <div class="section-content">
        <div>
          <div class="w-50 d-flex justify-content-between mb-1">
            <span>Total</span>
            <span class="fw-bold">{{details.total}}</span>
          </div>
          <div class="w-50 d-flex justify-content-between mb-1">
            <span>Trip</span>
            <span class="fw-bold">{{details.trip}} mi</span>
          </div>
          <div class="w-50 d-flex justify-content-between mb-1">
            <span>Rate / mile</span>
            <span class="fw-bold">
            {{details.rate}}
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="#6c757d" class="bi bi-exclamation-circle" viewBox="0 0 16 16">
              <circle cx="8" cy="8" r="8" fill="#6c757d"/>
              <path d="M7.002 11a1 1 0 1 0 2 0 1 1 0 0 0-2 0zm.93-7.481c-.58 0-1.057.498-.962 1.06l.35 3.507c.03.272.253.498.528.498h1.044c.275 0 .498-.226.528-.498l.35-3.507c.094-.562-.383-1.06-.963-1.06h-1.874z" fill="#fff"/>
            </svg>
          </span>
          </div>
        </div>
        <div class="detail-title-rate mt-5">
          <span>MARKET RATES Powered by DAT iQ</span>
          <span class="text-primary">RATE VIEW</span>
        </div>
        <div class="spot-and-contracts-rates mt-4">
          <span class="detail-title-rate">SPOT AND CONTRACTS RATES</span>
          <div class="d-flex justify-content-center align-items-center h-100">
            <span>Retrieving market rate data...</span>
          </div>
        </div>
      </div>
    </div>
    <div class="company w-100">
      <div class="details-title">
        <span>COMPANY</span>
        <span class="text-primary">VIEW IN DIRECTORY</span>
      </div>
      <div class="section-content">
        <div class="d-flex justify-content-between align-items-center">
          <span>{{details.company.name}}</span>
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24">
            <circle cx="12" cy="12" r="10" stroke-width="2" fill="lightgrey" />
            <path d="M8 10l4 4 4-4" stroke="black" stroke-width="2" fill="none" />
          </svg>
        </div>
        <p class="text-primary mt-4">{{details.company.email}}</p>
        <div class="d-flex justify-content-between align-items-center">
          <span>{{details.company.address}}</span>
          <div class="w-50">
            <div class="d-flex justify-content-between w-75 mb-1">
              <span>Credit Score</span>
              <span>{{details.company.score}}</span>
            </div>
            <div class="d-flex justify-content-between w-75">
              <span>Days to Pay</span>
              <span>{{details.company.daysToPay}}</span>
            </div>
          </div>
        </div>
        <div class="mt-5">
          <p class="fw-bold mb-2">LOAD RESOURCES</p>
          <div class="w-100 d-flex justify-content-between mb-1">
            <span>Factor with</span>
            <span class="fw-bold fs-6">{{details.company.factorWith}}</span>
          </div>
          <div class="w-100 d-flex justify-content-between">
            <span>Insurance</span>
            <span class="fw-bold fs-6">{{details.company.insurance}}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps } from 'vue'

const props = defineProps({
  details: {
    type: Object,
    default: Object
  },
  dates: {
    type: Array,
  }
});

const { details } = props

</script>

<style scoped>
.section-content {
  margin: 0 15px;
}
.details-title {
  background: lightgray;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;
  margin-right: 15px;
  padding: 0 5px;
}
.detail-title-rate {
  background: lightgray;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  padding: 0 5px;

}
.timeline {
  display: flex;
  flex-direction: column;
  position: relative;
}
.timeline::before {
  content: '';
  position: absolute;
  left: 4px;
  top: 0;
  height: 70%;
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
  align-items: flex-start;
  margin-bottom: 20px;
  position: relative;
}
.circle {
  width: 12px;
  height: 12px;
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
.date {
  margin-top: -10px;
  margin-left: 22px;
  color: gray;
  font-size: 0.9em;
}
.spot-and-contracts-rates {
  border: 3px solid lightgray;
  height: 300px;
}
</style>
