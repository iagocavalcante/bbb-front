<template>
  <div id="app">
    <h1>Contador de Estalecas - BBB20</h1>
    <ul>
      <li v-for="brother in orderedBrothers()" :key="brother.id">
        <div class="brother__pic">
          <span v-if="brother.status === 'OUT'" :style="isOut(brother)"></span>
          <img :src="brother.picture" alt="">
        </div>
        <div class="brother__info">
          <p>
            Nome: {{brother.name}}
          </p>
          <p>
            Estalecas: {{brother.balance}}
          </p>
          <p>
            Grupo: {{brother.group}}
          </p>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  data: () => ({
    brothers: [],
    isMounted: false
  }),
  methods: {
    isOut (brother) {
      return {
        backgroundColor: brother.status === 'OUT' ? '#fefefeA6' : '',
        width: '15em',
        height: '15em',
        position: 'absolute',
        borderRadius: '50%'
      }
    },
    orderedBrothers () {
      return this.brothers.sort((a, b) => (-1 * a.balance) - (-1* b.balance))
    }
  },
  created () {
    this.isMounted = true
    axios.get(`${process.env.VUE_APP_HOST}/bbb`)
      .then(response => {
        this.brothers = response.data
      })
      .catch(error => {
        console.log(error)
      })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}

* {
  color: white;
  background-color: #2c3e50;
}

h1 {
  text-align: center;
}

ul {
  list-style: none;
  display: grid;
  grid-template-columns: auto auto auto;
  text-align: center;
}

li {
  grid-template-rows: auto auto;
}

img {
  background-color: transparent;
  width: 15em;
  height: 15em;
  border-radius: 50%;
}
</style>
