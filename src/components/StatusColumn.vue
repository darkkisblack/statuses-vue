<template>
  <div class="status-column">
    <h2>{{ statusName }}</h2>
    <div class="deal-cards">
      <div v-for="deal in deals" :key="deal.OFFER_ID" class="deal-card">
        <p>{{ deal.OFFER_FIRST_RESPONSIBLE }}</p>
        <p>#{{ deal.OFFER_ID }}</p>
        <p>{{ deal.OFFERS_TYPE_NAME || '-' }}</p>
        <p>{{ deal.SEGMENT_NAME }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    statusName: String,
    deals: Array,
  },
};
</script>

<style scoped>
.status-column {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.deal-cards {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.deal-card {
  background-color: #f0f0f0;
  padding: 10px;
}
</style>
