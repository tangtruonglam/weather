<template>
  <div class="main-agileits">
    <h1>Weather Report Widget</h1>
    <Weather />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import Weather from '@/components/Weather.vue'; // @ is an alias to /src
@Component({
  components: {
    Weather,
  },
})
export default class Home extends Vue {}
</script>

