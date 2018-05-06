<template>
  <transition name="slide-fade">
    <b-card v-if="showData"
            :border-variant="cryptoData.colorByVariation"
            :header="cryptoData.fullName"
            :header-border-variant="cryptoData.colorByVariation"
            :header-bg-variant="cryptoData.colorByVariation"
            header-text-variant="white"
            align="center">
      <p class="card-text">
        <strong>Price:</strong> R$ {{ cryptoData.quotes.BRL.price.toLocaleString('pt-BR', { maximumFractionDigits: 2 }) }} <br />
        <strong>Rank:</strong> {{ cryptoData.rank }}º
      </p>
      <h4>Price variation:</h4>
      <p class="card-text">
        <strong>Last 24h:</strong> <b-badge :variant="cryptoData.colorByVariation"> {{ cryptoData.quotes.BRL.percent_change_24h }}% </b-badge> <br />
        <strong>Last 7d:</strong> <b-badge :variant="cryptoData.colorByVariationDays"> {{ cryptoData.quotes.BRL.percent_change_7d }}% </b-badge>
      </p>
    </b-card>
  </transition>
</template>

<script>
import axios from 'axios';

export default {
  name: 'crypto-data',

  props: ['crypto'],

  data() {
    return {
      showData: false,
      cryptoData: {},
    };
  },

  watch: {
    crypto(newVal) {
      const cm = this;

      if (newVal === 0) {
        cm.showData = false;
        return;
      }

      axios.get(`https://api.coinmarketcap.com/v2/ticker/${newVal}/?convert=BRL`).then((res) => {
        const data = res.data.data;

        cm.cryptoData = data;

        cm.cryptoData.colorByVariation = (data.quotes.BRL.percent_change_24h < 0) ? 'danger' : 'success';
        cm.cryptoData.colorByVariationDays = (data.quotes.BRL.percent_change_7d < 0) ? 'danger' : 'success';
        cm.cryptoData.fullName = `${data.name} (${data.symbol})`;

        cm.showData = true;
      }, () => {
        cm.showData = false;
        // eslint-disable-next-line
        alert('An error occurred while searching for crypto.');
      });
    },
  },

};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .3s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active em versões anteriores a 2.1.8 */ {
  transform: translateX(10px);
  opacity: 0;
}
</style>
