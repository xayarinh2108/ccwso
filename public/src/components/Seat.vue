<template>
    <button v-bind:id="seats_arr[seat_index].label"
            v-bind:style="{'background-color': color}"
            v-on:click="setSeatSelected(seat_index)"
            v-bind:class="{'hvr-shrink': seats_arr[seat_index].available}"
            v-bind:disabled="!seats_arr[seat_index].available">
        {{seats_arr[seat_index].label}}
    </button>
</template>

<script>
    import EventBus from '../bus.js'

    export default {
        name: "Seat",
        props: {
            seats_arr: Array,
            seat_index: Number
        },
        data () {
            return {
                color: ''
            }
        },
        created: function(){
            this.getSeatColor(this.seat_index);
            this.initSeatSelect(this.seat_index);
        },
        methods: {
            getSeatColor(index){
                let seatAvailable = this.seats_arr[index].available;
                let seatSelected = this.seats_arr[index].selected;
                if(seatAvailable){
                    if(seatSelected) this.color = '#8EE4AF'; // light green
                    else return this. color = '#FFFFFF'; // white
                } else return this.color = '#d3d3d3' ; //grey
            },
            initSeatSelect(index){
                this.seats_arr[index].selected = false;
            },
            setSeatSelected(index){
                this.seats_arr[index].selected = !this.seats_arr[index].selected;
                this.getSeatColor(index);
                EventBus.$emit('seat-selected', this.seats_arr[index]);
            }
        }
    }
</script>

<style scoped>

    @import '../../hover.css';

    button {
        height: 75%;
        width: 75%;
        border-radius: 15px;
    }

    button:focus {
        outline: 0;
        border: 1px black solid;
    }
</style>