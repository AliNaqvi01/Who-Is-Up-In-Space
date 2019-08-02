<template>
    <div>
        <div v-if="loadNames" class="animated bounce">
            <div v-for="(i, index) in namesArray" :index="index">
                 <vue-typer class="vueTyper" 
                 id="nameType"
                 :preTypeDelay='500' 
                 :repeat=0 
                 :text='namesArray[index].name'>
                </vue-typer>
            </div>
            
        </div>
        <a 
          id="buttonSlide1"  
          @click="this.$emit('ChangeView', $refs.fullpage.api.moveSlideLeft())" 
          v-bind:class="{animated: animate, bounce: animate}"
          class="hbtn hb-fill-middle2">
          <div id="buttonText">ISS Location</div>
          </a>
    </div>
</template>

<script>

export default {
    props: ['namesArray', 'loadNames'],
    data: {
        index: 0,


    },
    components: {


    },
    mounted: function(){
        console.log("HEY" + this.namesArray[2])
        console.log(this.namesArray)
        console.log("HELLLLLLLO" + this.loadNames)


    },
    methods: {
        moveToSlide2(event){
            this.$emit('moveToSlide2',)


        }


    }

}
</script>

<style scoped>

.vue-typer .custom.char.typed{
    color: gray;


}

#nameType{
    font-size: 45px;



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

