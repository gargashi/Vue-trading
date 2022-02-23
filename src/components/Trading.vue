<template>
  <div>Ashish Garg</div>
  <!-- <div v-for="price in priceDetail" :key="price">{{ price["1. open"] }}</div> -->
  <div>{{ tradingYValues }}</div>
  <GChart />
</template>

<script>
import "buefy/dist/buefy.css";
import { GChart } from "vue-google-charts";
import axios from "axios";

export default {
  data() {
    return {
      priceDetail: {},
      tradingXValues: [],
      tradingYValues: [],
    };
  },
  components: {
    GChart,
  },
  mounted() {
    this.setup()
      .then((resp) => {
        this.priceDetail = resp.data["Time Series (5min)"];
        console.log(this.priceDetail);
        for (var key in resp.data["Time Series (5min)"]) {
          this.tradingXValues.push(key);
          this.tradingYValues.push(
            resp.data["Time Series (5min)"][key]["1. open"]
          );
        }
      })
      .catch((error) => {
        return error;
      });
  },
  methods: {
    async setup() {
      const baseURI =
        "https://www.alphavantage.co/query?function=TIME_SERIES_INTRADAY&symbol=IBM&interval=5min&apikey=H2B6TNTJQPJUYI8C";
      const headers = {
        "Content-Type": "application/json",
      };
      const resp = axios.get(baseURI, {
        headers,
      });
      return resp;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
