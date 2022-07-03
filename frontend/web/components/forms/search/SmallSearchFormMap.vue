<template>
  <v-form ref="search" v-model="valid" lazy-validation class="search-form" @submit.prevent="submit">
      <div class="form-container">
        <city-select-header-form ref="citySelect" v-model="city" @input="onCityChange"></city-select-header-form>
        <input value="26.04 - 01.05" type="text" class="inputs__search-form bordered">
        <input value="3 гостя, 1 питомец" type="text" class="inputs__search-form">
    </div>
  </v-form>
</template>

<script>

import { mapMutations } from "vuex";
// import CitySelect from "../../controls/selects/CitySelect";
import CitySelectHeaderForm from "../../controls/selects/CitySelectHeaderForm";
const DATE_FORMAT = 'D.MM.YYYY'

export default {
  name: 'SmallSearchForm',
  components: {CitySelectHeaderForm},

  props: {
    short: {
      type: Boolean,
      default: false,
    },
  },
  data: (vm) => ({
    valid: false,

    dateFrom: null,
    dateTo: null,
    city: vm.$route.params.city,

    dateFromFormatted: !vm.dateFrom ? '' : vm.formatDate(vm.dateFrom),
    dateToFormatted: !vm.dateTo ? '' : vm.formatDate(vm.dateTo),
    menuDateFrom: false,
    menuDateTo: false,

    showCityHint: false,
  }),
  computed: {
    computedDateFormatted() {
      return this.formatDate(this.date)
    },
  },

  watch: {
    dateFrom(val) {
      this.dateFromFormatted = this.formatDate(this.dateFrom)
    },
    dateTo(val) {
      this.dateToFormatted = this.formatDate(this.dateTo)
    },
  },

  methods: {
    ...mapMutations({
      'setCurrentCity' : 'web/geo/setCity'
    }),
    onCityChange(value){
      this.showCityHint = false;
      this.setCurrentCity(value);
      if(this.short){
        this.submit();
      }
    },

    submit() {
      if (!this.city) {
        this.showCityHint = true
        this.$refs.citySelect.activateMenu();
        return
      }
      const url = `/search/${this.city}/`
      this.$router.push(url)
    },
    formatDate(date) {
      if (!date) return null
      return this.$dayjs(date).format(DATE_FORMAT)
    },
    parseDate(date) {
      if (!date) return null
      return this.$dayjs(date, DATE_FORMAT).format('YYYY-MM-DD')
    },
  },
}
</script>

<style scoped>
input{
  outline: none;
}

.inputs__search-form{
  height: 1.510vw;
  background-color: #FFFBF9;
  text-align: center;
  width: 9.479vw;
  margin-top: .5vw;
}

.bordered{
  border-left: .1vw solid #AFAFB0;
  border-right: .1vw solid #AFAFB0;
}

.form-container{
  width: 31.875vw;
  height: 2.708vw;
  display: flex;
  border-radius: 0.625vw;
  background: #FFFBF9;
}


</style>
