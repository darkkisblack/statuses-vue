<template>
  <div class="app">
    <status-column
      v-for="status in sortedStatuses"
      :key="status.STATUS_ID"
      :statusName="status.TITLE"
      :deals="status.deals"
    ></status-column>
  </div>
</template>

<script>
import axios from 'axios';
import StatusColumn from './components/StatusColumn.vue';

export default {
  components: {
    StatusColumn,
  },
  data() {
    return {
      statuses: [],
      deals: [],
    };
  },
  computed: {
    sortedStatuses() {
      return [...this.statuses].sort((a, b) => a.SORT - b.SORT);
    },
  },
  mounted() {
    this.fetchStatuses();
  },
  methods: {
    async fetchStatuses() {
      try {
        const response = await axios.get(
          'https://nastintesthodl.stocrm.ru/api/external/v1/offer/all_statuses?SID=10813_0c0a9a2f86eab09196705a274378b64a&BOARD_ID=1843',
        );
        const responseData = response.data.RESPONSE;
        this.statuses = Object.entries(responseData).map(([, value]) => ({ ...value }));

        await this.fetchDeals();
      } catch (error) {
        this.handleError(error);
      }
    },
    async fetchDeals() {
      try {
        const response = await axios.get(
          'https://nastintesthodl.stocrm.ru/api/external/v1/offers/get_from_filter?SID=10813_0c0a9a2f86eab09196705a274378b64a&PAGE=1&LIMIT=9999&REQUIRED_FIELDS[0]=',
        );
        const responseData = response.data.RESPONSE.DATA;
        this.deals = Array.from(responseData);
        this.mapDealsToStatuses();
      } catch (error) {
        this.handleError(error);
      }
    },
    mapDealsToStatuses() {
      this.statuses = this.statuses.map((status) => {
        const { STATUS_ID } = status;
        return {
          ...status,
          deals: this.deals.filter((deal) => deal.OFFER_STATUS_ID === STATUS_ID),
        };
      });
    },
    handleError(error) {
      console.error(error);
    },
  },
};
</script>

<style>
.app {
  display: flex;
  gap: 20px;
  padding: 20px;
}
</style>
