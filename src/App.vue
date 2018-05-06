<template>
  <div id="app">
    <b-container>
      <b-row class="justify-content-md-center">
        <b-col md="12">
          <h1>Crypto Change</h1>
        </b-col>
        <b-col lg="6">
          <v-select :on-change="changeCrypto" :options="cryptos" placeholder="Cryptocurrency..." class="select-crypto"></v-select>
          <crypto-data :crypto="cryptoId" :error="errorMessage"></crypto-data>
        </b-col>
      </b-row>
    </b-container>
    <div class="credits">
      <span>
        Made with <icon name="heart" scale="0.75"></icon> by <a href="https://www.github.com/adrcav" target="_blank">adrcav</a>
      </span>
    </div>
  </div>
</template>

<script>
import vSelect from 'vue-select';
import axios from 'axios';
import CryptoData from './components/Data';

export default {
  name: 'app',

  components: {
    CryptoData,
    vSelect,
  },

  data() {
    return {
      cryptos: [],
      cryptoId: 0,
    };
  },

  methods: {
    changeCrypto(val) {
      if (val !== null) {
        this.cryptoId = val.id;
      } else {
        this.cryptoId = 0;
      }
    },
  },

  mounted() {
    const cm = this;
    axios.get('https://api.coinmarketcap.com/v2/listings/').then((res) => {
      res.data.data.forEach((element) => {
        cm.cryptos.push({
          id: element.id,
          label: element.name,
        });
      });
    }, () => {
      // eslint-disable-next-line
      alert('An error occurred while searching for cryptos.');
    });
  },

};
</script>

<style>
body {
  background: #f5f8fa;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #333;
  margin-top: 60px;
}

.select-crypto {
  margin: 10px 0 15px;
}

.credits {
  position: fixed;
  bottom: 5px;
  left: 0;
  width: 100%;
  text-align: center;
  color: #aaa;
}
.credits a, a:focus {
  color: #aaa;
  text-decoration: none;
  font-weight: bold;
}

</style>
