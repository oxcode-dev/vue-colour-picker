# Vue-color-picker

Pickers Input using Vue-color Sketch, Compact & Chrome Colour pickers.

[Demo](https://ecl7n.csb.app/)
---

# Installation
```
    npm install vue-colour-picker
```

# Import
```
import { ColourPicker } from 'vue-colour-picker'

new Vue({
  components: {
    'colour-picker': ColourPicker
  },

  data(){
      return{
          colour: '#000000'
      }
  }
})
```

# Usage
For Chrome Colour Picker
```
    <colour-picker
        v-model="colour"
        :value="colour"
        label="Pick Colour"
        picker="chrome" />

```
For Compact Colour Picker
```
    <colour-picker
        v-model="colour"
        :value="colour"
        label="Pick Colour"
        picker="compact" />

```

For Sketch Colour Picker
```
    <colour-picker
        v-model="colour"
        :value="colour"
        label="Pick Colour"
        picker="sketch" />

```