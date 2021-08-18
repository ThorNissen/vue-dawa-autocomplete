<script>
import Vue from 'vue';
import VueDawaAutocomplete from '@/vue-dawa-autocomplete.vue';

export default Vue.extend({
  name: 'ServeDev',
  components: {
    VueDawaAutocomplete
  }
});
</script>

<template>
  <div id="app">
    <vue-dawa-autocomplete />
  </div>
</template>
