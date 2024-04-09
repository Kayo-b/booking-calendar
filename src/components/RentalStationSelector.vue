<template>
    <select @change="stationChanged($event.target.value)">
      <option v-for="station in stations" :key="station.id" :value="station.id">
        {{ station.name }}
      </option>
    </select>
  </template>
  
  <script>
  export default {
    props: {
      stations: Array
    },
    methods: {
      stationChanged(stationId) {
        this.$emit('station-selected', stationId);
      }
    }
  };
  </script>
  