<template>
    <div v-if="loadMap">
        <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.2/animate.min.css">
        <gmap-map id="map" class='animated bounceInUp' ref="mymap" :center="center" :zoom="2">
            <gmap-info-window 
                :options="infoOptions" 
                :position="infoPosition" 
                :opened="infoOpened"
                @closeclick="infoOpened=false">
            </gmap-info-window>
            <gmap-marker 
                v-for="(item, key) in coordinates" 
                :key="key" :position="getPosition(item)" 
                :clickable="true"
                @click="toggleInfo(item, key)" />
        </gmap-map>
    </div>
</template>

<script>

export default {
    props: ['coordinate', 'loadMap'],
    data() {
        return {
            center: {
                lat: 0,
                lng: 0
            },
            coordinates: {
                0: {
                    lat: 30,
                    lng: 30,
                },
            },
            infoPosition: null,
            infoContent: null,
            infoOpened: false,
            infoCurrentKey: null,
            infoOptions: {
                pixelOffset: {
                    width: 0,
                    height: -35
                }
            },
        }
    },
    methods: {
        getPosition: function (marker) {
            return {
                lat: parseFloat(marker.lat),
                lng: parseFloat(marker.lng)
            }
        },
        toggleInfo: function (marker, key) {
            this.infoPosition = this.getPosition(marker)
            this.infoContent = marker.full_name
            if (this.infoCurrentKey == key) {
                this.infoOpened = !this.infoOpened
            } else {
                this.infoOpened = true
                this.infoCurrentKey = key
            }
        }
    },
    mounted: function () {
        this.$nextTick(function () {
            window.setInterval(() => {
                this.coordinates = {
                    0: {
                        lat: this.coordinate.iss_position.latitude,
                        lng: this.coordinate.iss_position.longitude,
                    },
                }
            }, 1000);
        })
    },
}

</script>

<style scoped>

#map {
    width: 80%;
    height: 70vh;
    display: block;
    margin-left: auto;
    margin-right: auto;
    animation-duration: 1s;
    animation-delay: 0.3s;
    margin-bottom: 25px;
    border-style: solid;
    border-width: 5px;
    border-color: gray;
    border-radius: 3px;
}

.animated {
    padding-bottom: 1%;
}

</style>

