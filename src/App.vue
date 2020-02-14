<template>
  <div id="app">
    <h1>Contador de Estalecas - BBB20</h1>
    <ul>
      <li v-for="brother in brothers" :key="brother.id">
        <div class="brother__pic">
          <img v-if="brother.lider" class="brother__icon" src="https://gshow.especiaisgshow.globo/realities/bbb20/central-de-monitoramento/static/media/liderIcon.9af0d9f6.svg" alt="">
          <img v-if="brother.anjo" class="brother__icon" src="https://gshow.especiaisgshow.globo/realities/bbb20/central-de-monitoramento/static/media/anjoIcon.c77215ce.svg" alt="">
          <img v-if="brother.monstro" class="brother__icon" src="./assets/scary.svg" alt="">
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
    <div>Icons made by <a href="https://www.flaticon.com/authors/smashicons" title="Smashicons">Smashicons</a> from <a href="https://www.flaticon.com/" title="Flaticon">www.flaticon.com</a></div>
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
    }
  },
  created () {
    this.isMounted = true
    axios.get(`${process.env.VUE_APP_HOST}/bbb`)
      .then(response => {
        this.brothers = response.data.sort((a, b) => (-1 * a.balance) - (-1* b.balance))
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
  padding: 2em
}

img {
  background-color: transparent;
  width: 15em;
  height: 15em;
  border-radius: 50%;
}

@media (max-width: 700px) {
  ul {
    grid-template-columns: auto;
  }
}

.brother__icon {
  position: absolute;
  width: 4em;
  height: 4em;
}
</style>
