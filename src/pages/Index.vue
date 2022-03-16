<template>
  <q-page class="flex flex-center">
  <div class=" column items-center">
    <h2> {{name}} </h2>
    <q-img :src="url" width="250px" />

  <div class="row">
    <div class="col-4 q-mr-md">
    <span>Type</span>
      <div v-for="(type, index) in pokeTypes" :key="index" class="q-pt-sm">
        <q-badge rounded  color="primary" :label="type" />
      </div>
    </div>
    <div class="col-4 q-ml-md">
    <span>Ability</span>
      <div v-for="(ability, idx) in pokeAbilities" :key="idx" class="q-pt-sm">
        <q-badge rounded  color="secondary" :label="ability" />
      </div>
    </div>
  </div>
  </div>
  <div class="row justify-around full-width">
    <q-input filled v-model="search" label="Digite nome Pokemon" />
    <q-btn color="purple" label="Pesquisar" @click="getPokemon"/>
  </div>
  <div class="row justify-between absolute full-width container-arrows">
    <q-icon
      name="far fa-arrow-alt-circle-left"
      color="primary"
      size="50px"
      class="q-ml-sm cursor-pointer"
      @click="getPokemon(id - 1)"
      >
    <q-tooltip>
      <span>
        Anterior
      </span>
      </q-tooltip>
    </q-icon>
    <q-icon
      name="far fa-arrow-alt-circle-right"
      color="primary"
      size="50px"
      class="q-mr-sm cursor-pointer"
      @click="getPokemon(id + 1)"
      >
    <q-tooltip>
      <span>
        Próxima
      </span>
      </q-tooltip>
    </q-icon>
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
      id: null,
      search: 'nidoqueen',
      pokeTypes: [],
      pokeAbilities: []
    }
  },

  async beforeMount(){
    await this.getPokemon()
  },

  methods: {
      getPokemon(id) {
        api
        .get(id > 0 ? `/pokemon/${id}/` : `/pokemon/${this.search}/`)
        .then((response) => {
          this.pokeTypes = []
          this.pokeAbilities = []

          //lidar com o sucesso
          this.id = response.data.id;
          this.name = response.data.name;
          this.url = response.data.sprites.other.dream_world.front_default;
          this.search = response.data.name;

          const nameTypes = response.data.types
          nameTypes.forEach(pokemonType => {
            this.pokeTypes.push(pokemonType.type.name)
          })

          const nameAbilities = response.data.abilities
          nameAbilities.forEach(pokemonAbility => {
            this.pokeAbilities.push(pokemonAbility.ability.name)
          })
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
          message: 'Pokemon não encontrado! Você escreveu errado!'
        })
      },
    },
  }
</script>

