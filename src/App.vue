<template>
    <div>
        <button @click="goToPreviousWeek">Previous Week</button>
        <button @click="goToNextWeek">Next Week</button>
        <div></div>
        <RentalStationSelector :stations="stations" @station-selected="stationSelected" />
        <WeekView :startDate="startDate" :selectedStationBookings="selectedStationBookings" />
    </div>
  </template>
  
  <script>
  import WeekView from './components/WeekView.vue';
  import RentalStationSelector from './components/RentalStationSelector.vue';
  
  export default {
        components: {
            RentalStationSelector,
            WeekView

        },
        data() {
            return {
                startDate: new Date("2021-03-13T22:04:19.032Z"),
                stations: [],
                selectedStationBookings: []
            }
        },
        mounted() {
            this.adjustStartDateToBeginningOfWeek();
            this.fetchStationsBookingData().then(() => {
                this.stationSelected("1")
            });
        },
        methods: {
            adjustStartDateToBeginningOfWeek() {
                const dayOfWeek = this.startDate.getDay();
                console.log(dayOfWeek)
                const diff = this.startDate.getDate() - dayOfWeek + (dayOfWeek === 0 ? -6 : 1); 
                console.log(this.startDate)
                this.startDate.setDate(diff);
            },
            goToPreviousWeek() {
                this.changeWeekBy(-7);
            },
            goToNextWeek() {
                this.changeWeekBy(7);
            },
            changeWeekBy(days) {
                const newDate = new Date(this.startDate.valueOf()); 
                newDate.setDate(this.startDate.getDate() + days); 
                this.startDate = newDate; 
                this.adjustStartDateToBeginningOfWeek();
            },
            async fetchStationsBookingData() {
                // Added async fetch to be able to call this.stationSelected("1")
                // after fetching data and have the station data readly available;
                try {
                    const response  = await fetch('https://605c94c36d85de00170da8b4.mockapi.io/stations')
                    const data = await response.json();
                    this.stations = data;
                    } catch (error) {
                        console.error("Failed to fetch data", error)
                    }
            },

            stationSelected(stationId) {
              const station = this.stations.find(s => s.id === stationId);
              this.selectedStationBookings = station ? station.bookings : [];
              console.log(this.selectedStationBookings, "<###")
            }

        }
  }
  </script>
  