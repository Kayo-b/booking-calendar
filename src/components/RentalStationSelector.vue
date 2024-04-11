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
        const station = this.stations.find(station => station.id === stationId);
        this.$emit('station-selected', stationId, station ? station.name : null);
      }
    },
    watch: {
      stations: {
        immediate: true,
        handler(stations) {
          if (stations.length > 0) {
            this.stationChanged(stations[0].id);
          }
        }
      }
    }
  };
  </script>
  