<template>
<div @click="hide" :class="{ modalBackground: isVisible }"></div>
    <div class="main-container" :style="{ display: isVisible ? 'block' : 'none'}">
        <div class="booking-detail-view">
            <button @click="hide">Back</button>
            <div class="booking-detail-view__header">
                <h1>Booking Details</h1>
            </div>
            <div class="booking-detail-view__content">
                <div class="booking-detail-view__content__booking">
                    <div class="booking-detail-view__content__booking__label">Booking ID:</div>
                    <div class="booking-detail-view__content__booking__value">{{ booking.id }}</div>
                </div>
                <div class="booking-detail-view__content__booking">
                    <div class="booking-detail-view__content__booking__label">Customer Name:</div>
                    <div class="booking-detail-view__content__booking__value">{{ booking.customerName }}</div>
                </div>
                <div class="booking-detail-view__content__booking">
                    <div class="booking-detail-view__content__booking__label">Start Date:</div>
                    <div class="booking-detail-view__content__booking__value">{{ booking.startDate }}</div>
                </div>
                <div class="booking-detail-view__content__booking">
                    <div class="booking-detail-view__content__booking__label">End Date:</div>
                    <div class="booking-detail-view__content__booking__value">{{ booking.endDate }}</div>
                </div>
                <div class="booking-detail-view__content__booking">
                    <div class="booking-detail-view__content__booking__label">Duration: </div>
                    <div class="booking-detail-view__content__booking__value">{{ Math.floor(( new Date(booking.startDate).getTime() - new Date(booking.endDate).getTime()) / (1000 * 3600 * 24) * -1)}} Days</div>
                </div>
                <div class="booking-detail-view__content__booking">
                    <div class="booking-detail-view__content__booking__label">Station:</div>
                    <div class="booking-detail-view__content__booking__value">{{ stationName }}</div>
                </div>
            </div>
        </div>
    </div>

</template>

  <script>
    export default {
        data() {
            return {
                isVisible: false
            }
        },
        props: {
            booking: Object,
            clicked: Boolean,
            stationName: String
        },
        watch: {
            clicked() {
                this.show();
            }
        },
        methods: {
            show() {
                // console.log("show")
                // this.main-container.style.visibility = 'visible';
                // this.$el.style.display = 'block';
                this.isVisible = !this.isVisible
            },
            hide() {
                // this.$el.style.visibility = 'hidden';
                this.isVisible = !this.isVisible;
            }
        
        }
    }
    </script>
<style scoped>
    .modalBackground {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: rgba(0, 0, 0, 0.5);
        visibility: visible;
        
    }
    .main-container {
        display: flex;
        position: absolute;
        justify-content: center;
        align-items: center;
        top: 10%;
        right: 35%;
        background-color: black; 
        /* visibility: hidden; */
        /* height: 100vh; */
        /* border: 1px solid #ff0000; */
    }
    .booking-detail-view {
        width: 300px;
        border: 1px solid #ccc;
        border-radius: 5px;
        padding: 10px;
    }
    .booking-detail-view__header {
        text-align: center;
        margin-bottom: 10px;
    }
    .booking-detail-view__content__booking {
        display: flex;
        margin-bottom: 5px;
    }
    .booking-detail-view__content__booking__label {
        font-weight: bold;
        width: 100px;
    }
    .booking-detail-view__content__booking__value {
        flex: 1;
    }

</style>