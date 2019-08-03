<template>
    <div>
        <div v-if="loadNames" class="animated bounce">
            <div v-for="(i, index) in namesArray" :index="index">
                <vue-typer 
                    class="vueTyper" id="nameType" 
                    :preTypeDelay='500' :repeat=0 
                    :text='namesArray[index].name'>
                </vue-typer>
            </div>

        </div>

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
    mounted: function () {
        console.log("HEY" + this.namesArray[2])
        console.log(this.namesArray)
        console.log("HELLLLLLLO" + this.loadNames)
    },
    methods: {
        moveToSlide2(event) {
            this.$emit('moveToSlide2', )
        }
    }
}
</script>

<style scoped>

.vue-typer .custom.char.typed {
    color: gray;
}

#nameType {
    font-size: 45px;
}

.animated {
    padding-bottom: 1%;
}

</style>

