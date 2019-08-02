<template>

    <div v-if="loadMap" >
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.2/animate.min.css">
        <gmap-map 
        id="map"
        class='animated bounceInUp' 
        ref="mymap" 
        :center="center" 
        :zoom="2">
            <gmap-info-window :options="infoOptions" :position="infoPosition" :opened="infoOpened" @closeclick="infoOpened=false">
            </gmap-info-window>
            <gmap-marker v-for="(item, key) in coordinates" :key="key" :position="getPosition(item)" :clickable="true" @click="toggleInfo(item, key)" />
        </gmap-map>
                <a 
          id="buttonSlide1"  
          @click="this.$emit('ChangeView', $refs.fullpage.api.moveSlideLeft())" 
          v-bind:class="{animated: animate, bounce: animate}"
          class="hbtn hb-fill-middle2">
          <div id="buttonText">Back</div>
          </a>
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


#map{
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

.hbtn {
      position: relative;
      box-sizing: border-box;
      display: inline-block;
      overflow: hidden;
      padding: 8px 5%;
      margin: 0px 3px 6px;
      text-align: center;
      border: 2px solid rgb(255, 255, 255);
      text-decoration: none;
      color: rgb(255, 255, 255);
      white-space: nowrap;
      z-index: 0;
} 
 

.hbtn i {
      padding-right: 8px;
     
} 
 

.hb-fill-middle2::before {
      position: absolute;
      content: "";
      background: rgb(255, 255, 255);
      transition-duration: 0.3s;
      z-index: -1;
      top: 0px;
      right: 0px;
      bottom: auto;
      left: auto;
      width: 0px;
      height: 100%;
      opacity: 1;
} 
 

.hb-fill-middle2:hover::before {
      width: 50%;
      height: 100%;
      opacity: 1;
} 
 

.hb-fill-middle2 {
      background: transparent;
      color: rgb(255, 255, 255);
      transition: color 0.3s ease 0s, background 0s ease 0s;
} 
 

.hb-fill-middle2:hover {
      color: rgb(0, 0, 0);
      background: rgb(255, 255, 255);
      transition: color 0.3s ease 0s, background 0s ease 0.3s;
} 
 

.hb-fill-middle2::after {
      position: absolute;
      content: "";
      background: rgb(255, 255, 255);
      transition-duration: 0.3s;
      z-index: -1;
      top: 0px;
      right: auto;
      bottom: auto;
      left: 0px;
      width: 0px;
      height: 100%;
      opacity: 1;
} 
 

.hb-fill-middle2:hover::after {
      width: 50%;
} 

#buttonSlide{
    padding-top: 100px !important; 


}
 
 #buttonText{
  color: white;
    font-family: 'Staatliches', cursive;
    font-size: 1.5em;
}
#buttonText:hover{
  color: black;
    font-family: 'Staatliches', cursive;
}

#buttonSlide1:hover > #buttonText{
    color: black;


}

.animated{
    padding-bottom: 1%;

    
}


</style>

