<template>
  <multiselect
    id="country-select"
    label="name"
    open-direction="below"
    :hideSelected="true"
    :options="countryOptions"
    :placeholder="placeholder"
    :selectLabel="selectLabel"
    :value="country"
    @input="updateCountryAction">
    <template v-slot:noResult>No country found. Check the spelling and try again.</template>
  </multiselect>
</template>

<script>
import Multiselect from 'vue-multiselect';
import { TravelDirection } from '@/constants/travel';
import { mapGetters, mapState, mapActions } from 'vuex';

export default {
  name: 'CountrySelect',
  components: { Multiselect },
  watch: {
    country(val) {
      const country = this.getCountryByCode(val.code);
      if (this.$route.params.country === country.slug) return;
      this.$router.push({ name: 'Country', params: { country: country.slug } });
    },
  },
  computed: mapState({
    ...mapGetters(['getCountryByCode']),
    ...mapState(['country', 'countryOptions', 'travelContext']),
    placeholder() {
      return this.travelContext === TravelDirection.Inbound
        ? 'Where are you traveling to?'
        : 'Where are you traveling from?';
    },
    selectLabel() {
      return 'ontouchstart' in document
        ? ''
        : 'Press enter to select';
    },
  }),
  methods: {
    ...mapActions(['updateCountryAction']),
  },
};
</script>

<style src="vue-multiselect/dist/vue-multiselect.min.css"></style>

<style>
.multiselect__tags {
  @apply pl-3 border rounded-md;
}

.multiselect__placeholder,
.multiselect__input {
  @apply pl-0 text-base leading-none;
}

.multiselect__single {
  @apply pl-0;
}

.multiselect__option--highlight,
.multiselect__option--highlight:after {
  @apply bg-primary;
}

.multiselect__option--selected.multiselect__option--highlight,
.multiselect__option--selected.multiselect__option--highlight:after {
  @apply bg-secondary;
}
</style>
