<template>
  <div id="app">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <link href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
    <link href="https://fonts.googleapis.com/css?family=Staatliches&display=swap" rel="stylesheet">
    <link type="text/css" src="https://cdnjs.com/libraries/bttn.css"/>
    <link type="text/css" src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/css/bootstrap.min.css"/>

    

        <link rel="stylesheet" href="https://cdnjs.com/libraries/bttn.css">

    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.2/animate.min.css">
    <full-page ref="fullpage" id="fullpage" :options="options" @on-leave="onLeave">
      <div class="section fullpage" id="intro">
        <div class="pop animated bounceInDown col-sm-12 col-md-12 col-lg-12">
          
          <vue-typer 
          id="introType" 
          repeat="0" 
          typeDelay="52"
          :text='`🌌 There are ${apiData.number.toString()} people in Space right now.`'>
          </vue-typer>
          
        </div>
      </div>
      <div class="section fullpage">
        <div class="slide">
          <names ref="foo" :namesArray="apiData.people" :loadNames='loadNames' @ChangeView='$event'> </names>
          
        </div>
      <div class="slide "> 
        <location :loadMap="loadMap" :coordinate="apiLocationData" > </location>
      </div>
    </div>
  </full-page>
</div>
  
</template>
    <script src="https://unpkg.com/vue-fullpage.js/dist/vue-fullpage.min.js"></script>

<script>

import Names from './components/Names.vue'
import Location from './components/Location.vue'


export default {
  name: 'app',
  data() {
    return {
      loadNames: false,
      loadMap: false,
      animate: false,
      apiData: {

      },
      apiLocationData: {
        x: 0

      },
      // These are properties from FullPage JS
      options: {
        licenseKey: 'YOUR_KEY_HERE',
        afterLoad: this.afterLoad,
        scrollOverflow: true,
        scrollHorizontally: true,
        scrollBar: false,
        menu: '#menu',
        navigation: true,
        navigationPosition: 'right',
        anchors: ['page1', 'page2', 'page3'],
                    afterLoad: this.afterLoad,
                    onLeave: this.onLeave,
                    onSlideLeave: this.onSlideLeave,


      }
    }
  },
  created: function () {
    // Call to get API data
    this.$http.get('http://api.open-notify.org/astros.json')
      .then(response => {
        return response.json()
      })
      .then(data => {
        this.apiData = data
        console.log(this.apiData)
      })
  },
  mounted: function () {
    // Timer calls ISS LOCATION API every second
    this.$nextTick(function () {
      window.setInterval(() => {
        this.$http.get('http://api.open-notify.org/iss-now.json')
          .then(response => {
            return response.json()
          })
          .then(data => {
            this.apiLocationData = data
            console.log(this.apiLocationData)
          })
      }, 1000);
    })
  },
  components: {
    "names": Names,
    "location": Location,
  },
  methods: {

    moveToSlide2(value){
      value



    },

    // Methods from FullPage JS
    afterLoad: function (origin, destination, direction) {
      //this.loadNames = true
       console.log(this.loadNames)
      console.log("After load sad");
     
    },
    onLeave: function (origin, destination, direction) {
      this.loadNames = true
      console.log("Leaving now!~!!!!!!!!!!!");
      console.log(destination);
      if(origin.index == 1 && direction =='right'){
			console.log("SLIDE");
		}
    },
    onSlideLeave: function( section, origin, destination, direction){
      this.loadMap = true
      console.log('SLIDE')

    },
    addSection: function (e) {
      var newSectionNumber = document.querySelectorAll('.fp-section').length + 1

      // creating the section div
      var section = document.createElement('div')
      section.className = 'section'
      section.innerHTML = `<h3>Section ${newSectionNumber}</h3>`

      // adding section
      document.querySelector('#fullpage').appendChild(section)

      // creating the section menu element
      var sectionMenuItem = document.createElement('li')
      sectionMenuItem.setAttribute('data-menuanchor', 'page' + newSectionNumber)
      sectionMenuItem.innerHTML = `<a href="#page${newSectionNumber}">Section${newSectionNumber}</a>`

      // adding anchor for the section
      this.options.anchors.push(`page${newSectionNumber}`)

      // we have to call `update` manually as DOM changes won't fire updates
      // requires the use of the attribute ref="fullpage" on the
      // component element, in this case, <full-page>
      // ideally, use an ID element for that element too
      this.$refs.fullpage.build()
    },

    removeSection: function () {
      var sections = document.querySelector('#fullpage').querySelectorAll('.fp-section')
      var lastSection = sections[sections.length - 1]

      // removing the last section
      lastSection.parentNode.removeChild(lastSection)

      // removing the last anchor
      this.options.anchors.pop()

      // removing the last item on the sections menu
      var sectionsMenuItems = document.querySelectorAll('#menu li')
      var lastItem = sectionsMenuItems[sectionsMenuItems.length - 1]
      lastItem.parentNode.removeChild(lastItem)
    }
  }
}

</script>

<style>

.pop{
  overflow-wrap: normal


}


#introType{
  font-size: 3em;
}

#buttonSlide1{
position: relative;
    right: 0;
    transform: rotate(0deg);


}

.vue-typer .custom.char.typed{
  color: white;
  font-family: 'Staatliches', cursive;
    display: inline-block;
    word-break:initial;
    white-space: wrap;
    overflow-wrap: break-word;


}

.vue-typer .custom.char.typed:after{

 content:"\a";
    white-space: pre;
}

  



.fullpage{
  background-image: url('./assets/IntroPhoto.jpg');

transform: translate(0px,-60px) !important;

}



#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

/* FULLPAGE JS CSS STYLES */

.fp-enabled body {
    margin: 0;
    padding: 0;
    overflow:hidden;

    /*Avoid flicker on slides transitions for mobile phones #336 */
    -webkit-tap-highlight-color: rgba(0,0,0,0);
}

.fp-section {
    position: relative;
    -webkit-box-sizing: border-box; /* Safari<=5 Android<=3 */
    -moz-box-sizing: border-box; /* <=28 */
    box-sizing: border-box;
}
.fp-slide {
    float: left;
}
.fp-slide, .fp-slidesContainer {
    height: 100%;
    display: block;
}
.fp-slides {
    z-index:1;
    height: 100%;
    overflow: hidden;
    position: relative;
    -webkit-transition: all 0.3s ease-out; /* Safari<=6 Android<=4.3 */
    transition: all 0.3s ease-out;
}
.fp-section.fp-table, .fp-slide.fp-table {
    display: table;
    table-layout:fixed;
    width: 100%;
}
.fp-tableCell {
    display: table-cell;
    vertical-align: middle;
    width: 100%;
    height: 100%;
}
.fp-slidesContainer {
    float: left;
    position: relative;
}

.fp-scrollable {
    overflow: hidden;
    position: relative;
}
.fp-scroller{
    overflow: hidden;
}
.iScrollIndicator{
    border: 0 !important;
}
.fp-notransition {
    -webkit-transition: none !important;
    transition: none !important;
}
#fp-nav {
    position: fixed;
    z-index: 100;
    margin-top: -32px;
    top: 50%;
    opacity: 1;
    -webkit-transform: translate3d(0,0,0);
}
#fp-nav.fp-right {
    right: 17px;
}
#fp-nav.fp-left {
    left: 17px;
}
.fp-slidesNav{
    position: absolute;
    z-index: 4;
    opacity: 1;
    -webkit-transform: translate3d(0,0,0);
    -ms-transform: translate3d(0,0,0);
    transform: translate3d(0,0,0);
    left: 0 !important;
    right: 0;
    margin: 0 auto !important;
}
.fp-slidesNav.fp-bottom {
    bottom: 17px;
}
.fp-slidesNav.fp-top {
    top: 17px;
}
#fp-nav ul,
.fp-slidesNav ul {
  margin: 0;
  padding: 0;
}
#fp-nav ul li,
.fp-slidesNav ul li {
    display: block;
    width: 14px;
    height: 13px;
    margin: 7px;
    position:relative;
}
.fp-slidesNav ul li {
    display: inline-block;
}
#fp-nav ul li a,
.fp-slidesNav ul li a {
    display: block;
    position: relative;
    z-index: 1;
    width: 100%;
    height: 100%;
    cursor: pointer;
    text-decoration: none;
}
#fp-nav ul li a.active span,
.fp-slidesNav ul li a.active span,
#fp-nav ul li:hover a.active span,
.fp-slidesNav ul li:hover a.active span{
    height: 12px;
    width: 12px;
    margin: -6px 0 0 -6px;
    border-radius: 100%;
 }
#fp-nav ul li a span,
.fp-slidesNav ul li a span {
    border-radius: 50%;
    position: absolute;
    z-index: 1;
    height: 4px;
    width: 4px;
    border: 0;
    background: #333;
    left: 50%;
    top: 50%;
    margin: -2px 0 0 -2px;
    -webkit-transition: all 0.1s ease-in-out;
    -moz-transition: all 0.1s ease-in-out;
    -o-transition: all 0.1s ease-in-out;
    transition: all 0.1s ease-in-out;
}
#fp-nav ul li:hover a span,
.fp-slidesNav ul li:hover a span{
    width: 10px;
    height: 10px;
    margin: -5px 0px 0px -5px;
}
#fp-nav ul li .fp-tooltip {
    position: absolute;
    top: -2px;
    color: #fff;
    font-size: 14px;
    font-family: arial, helvetica, sans-serif;
    white-space: nowrap;
    max-width: 220px;
    overflow: hidden;
    display: block;
    opacity: 0;
    width: 0;
    cursor: pointer;
}
#fp-nav ul li:hover .fp-tooltip,
#fp-nav.fp-show-active a.active + .fp-tooltip {
    -webkit-transition: opacity 0.2s ease-in;
    transition: opacity 0.2s ease-in;
    width: auto;
    opacity: 1;
}
#fp-nav ul li .fp-tooltip.fp-right {
    right: 20px;
}
#fp-nav ul li .fp-tooltip.fp-left {
    left: 20px;
}
.fp-auto-height.fp-section,
.fp-auto-height .fp-slide,
.fp-auto-height .fp-tableCell{
    height: auto !important;
}

.fp-responsive .fp-auto-height-responsive.fp-section,
.fp-responsive .fp-auto-height-responsive .fp-slide,
.fp-responsive .fp-auto-height-responsive .fp-tableCell {
    height: auto !important;
}

#introType {

  width: 100%; 
height: 50%; 
display: block; 
margin-left: auto; 
margin-right: auto;


}

/*Only display content to screen readers*/
.fp-sr-only{

    position: absolute;
    width: 1px;
    height: 1px;
    padding: 0;
    overflow: hidden;
    clip: rect(0, 0, 0, 0);
    white-space: nowrap;
    border: 0;
 
}




  

</style>
