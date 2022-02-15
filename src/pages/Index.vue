<template>
  <q-page class="flex flex-center">
  <div class=" column items-center">
    <h2> {{name}} </h2>
    <q-img :src="url" width="250px" />
  </div>
  <div class="row justify-around full-width">
    <q-input filled v-model="search" label="Digite nome Pokemon" />
    <q-btn color="purple" label="Pesquisar" @click="getPokemon"/>
  </div>
  <div class="row justify-between full-width">
    <q-icon
      name="far fa-arrow-alt-circle-left"
      color="primary"
      class="q-ml-sm cursor-pointer"
      style="font-size: 4rem"
    />
    <q-icon
      name="far fa-arrow-alt-circle-right"
      color="primary"
      class="q-mr-sm cursor-pointer"
      style="font-size: 4rem"
    />

  </div>
  </q-page>
</template>

<script>
import api from "../services/api";
export default {
  name: "PageIndex",

  data() {
    return {
      name: '',
      url: '',
      search: 'ditto',
    }
  },

  async beforeMount(){
    await this.getPokemon()
  },

  methods: {
      getPokemon() {
            api
        .get(`/pokemon/${this.search}/`)
        .then((response) => {
          //lidar com o sucesso
          console.log(response);
          this.name = response.data.name;
          this.url = response.data.sprites.other.dream_world.front_default;
          this.triggerPositive()
        })
        .catch((error) => {
          //lidar com o erro
          this.triggerNegative()
        })
        .then(() => {
          //sempre
        })
      },
      triggerPositive () {
        this.$q.notify({
          type: 'positive',
          position: "top",
          message: 'Pokemon encontrado!'
        })
      },
      triggerNegative () {
        this.$q.notify({
          type: 'negative',
          position: "top",
          message: 'Pokemon não encontrado! Tente novamente!'
        })
      },
    },
  }
</script>
