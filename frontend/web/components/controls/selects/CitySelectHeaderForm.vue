<template>
  <v-autocomplete
    ref="selectCity"
    :value="value"
    color="#00ACA2"
    flat
    solo
    class="rounded-lg select-city pa-0"
    :prepend-inner-icon="magnifer"
    label="Куда едем?"
    :items="items"
    @change="onCitySelectHandler"
  >
  </v-autocomplete>
</template>

<script>
import {mapActions} from "vuex";
import { mdiMagnify } from '@mdi/js';

export default {
  name: "CitySelect",
  props:{
    dense:{
      type: Boolean,
      default: false
    },
    value:{
      type: String,
      default: ''
    }
  },
  data(){
    return{
      magnifer: mdiMagnify,
      cities: [
        { url: 'moscow', title: 'Москва', count: 10000, country: { alfa2: 'RU'} },
        { url: 'rostov-on-don', title: 'Ростов-на-Дону', count: 10000, country: { alfa2: 'RU'} },
        { url: 'sochi', title: 'Сочи', count: 10000, country: { alfa2: 'RU'} },
      ],

    }
  },
  computed:{
    items(){
      return this.cities.map((item) => {
        return { value: item.url, text: item.title, count: 0, country: item?.country?.alfa2 }
      });
    }
  },
  mounted() {
    // this.getCities().then((response) => {
    //   this.cities = response.data;
    // });
  },
  methods:{
    ...mapActions({
        getCities: 'web/geo/cities'
    }),
    onCitySelectHandler(value) {
      this.$emit('input', value)
    },

    activateMenu(){
      this.$refs.selectCity.focus()
      this.$refs.selectCity.activateMenu()
      this.$refs.selectCity.isMenuActive = true
    }
  }
}
</script>

<style >
  .mdi-menu-down::before{
    content: '';
  }

  #input-15{
    text-align: center;
  }

  .v-input__slot{
    min-height: 2.708vw !important;
    box-shadow: none !important;
    background-color: #FFFBF9 !important;
    border-radius: .833vw !important;
  }
</style>
