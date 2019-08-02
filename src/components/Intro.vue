
<template>
    <div>

        <h2>
            <vue-typer :text='numberOfPeople'></vue-typer>
        </h2>
    </div>
</template>

<script>

import {
    VueTyper
} from 'vue-typer'

export default {
    props: ['numberOfPeople'],
    data() {
        return {

        }
    },
    components: {
        'vue-typer': VueTyper
    }
}

</script>
