<template>
    <div>
        <ColourPicker v-model="colour" :color="colour" picker="chrome" label="Color" />
    </div>
</template>

<script>
import ColourPicker from 'vue-colour-picker'
export default {
    name: 'Demo',
    data(){
        return{
            colour: '#000000'
        }
    }
}
</script>

<style></style>


