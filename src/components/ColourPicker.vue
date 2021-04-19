<template>
    <div>
        <label class="form-label" v-if="label">{{ label }}:</label>
        <div class="input-group color-picker flex" ref="colorpicker" style="width:100%">
            <input
                type="text" ref="input"
                v-bind="$attrs" class="color-input"
                @focus="showPicker()" @input="updateFromInput"
                placeholder="Pick colour..."
                v-if="!noInput"
            />

            <span class="input-group-addon color-picker-container">
                <span class="current-color" :style="colorBox()" @click="togglePicker()"></span>
            </span>

            <div style="position:relative">
                <chrome-picker :value="colors" @input="updateFromPicker" v-show="picker === 'chrome' && displayPicker" :class="noInput ? 'vc-left' : 'vc-right'" />
                <sketch-picker :value="colors" @input="updateFromPicker" v-show="picker === 'sketch' && displayPicker" :class="noInput ? 'vc-left' : 'vc-right'" />
                <compact-picker :value="colors" @input="updateFromPicker" v-show="picker === 'compact' && displayPicker" :class="noInput ? 'vc-left' : 'vc-right'" />
            </div>

        </div>

    </div>
</template>

<script>
import { Chrome } from 'vue-color'
import { Sketch } from 'vue-color'
import { Compact } from 'vue-color'
export default {
    components: {
        'chrome-picker': Chrome,
        'sketch-picker': Sketch,
        'compact-picker': Compact,
    },
    inheritAttrs: false,
    props: {
        noInput:{
            type: Boolean,
            default: () => false
        },
        color: {
            type: String,
            default: () => '',
        },
        label: {
            type: String,
            default: () => '',
        },
        picker:{
            type: String,
            default: () => 'chrome',
            validator: value => {
                 return ['chrome', 'compact', 'sketch'].indexOf(value) !== -1
            }
        },

    },
    data() {
        return {
            colors: {
                hex: '#000000',
            },
            colorValue: '',
            displayPicker: false,
            defaultColor: '#FF0000'
        }
    },

    methods: {
        colorBox(){
            let border = this.noInput ? '4px' : '0px 4px 4px 0px';
            return `background-color: ${this.colorValue};
                    border-radius: ${border}
                    `;
        },

        setColor(color) {
            this.updateColors(color);
            this.colorValue = color;
        },

        updateColors(color) {
            if(color.slice(0, 1) === '#') {
                this.colors = {
                    hex: color
                };
            }
            else if(color.slice(0, 4) === 'rgba') {
                let rgba = color.replace(/^rgba?\(|\s+|\)$/g,'').split(','),
                    hex = '#' + ((1 << 24) + (parseInt(rgba[0]) << 16) + (parseInt(rgba[1]) << 8) + parseInt(rgba[2])).toString(16).slice(1);
                this.colors = {
                    hex: hex,
                    a: rgba[3],
                }
            }
        },

        showPicker() {
            document.addEventListener('click', this.documentClick);
            this.displayPicker = true;
        },

        hidePicker() {
            document.removeEventListener('click', this.documentClick);
            this.displayPicker = false;
        },
        togglePicker() {
            this.displayPicker ? this.hidePicker() : this.showPicker();
        },

        updateFromInput() {
            this.updateColors(this.colorValue);
        },

        updateFromPicker(color) {
            this.colors = color;
            if(color.rgba.a === 1) {
                this.colorValue = color.hex;
            }
            else {
                this.colorValue = 'rgba(' + color.rgba.r + ', ' + color.rgba.g + ', ' + color.rgba.b + ', ' + color.rgba.a + ')';
            }
        },

        documentClick(e) {
            let el = this.$refs.colorpicker,
                target = e.target;
            if(el !== target && !el.contains(target)) {
                this.hidePicker()
            }
        }
    },

    watch: {
        colorValue(val) {
            if(val) {
                this.updateColors(val);
                this.$emit('input', val);
                //document.body.style.background = val;
            }
        }
    },

    mounted() {
        this.setColor(this.color || '#000000');
    }
}
</script>


<style>
    .vc-left{
        left: -39px;
    }
    .vc-right{
        right: 0;
    }
    .color-input{
        padding: .5rem;
        line-height: 1.5;
        display: block;
        width: 100%;
        border-width: 1px;
        color: #3d4852;
        background-color: #fff;
        border-radius: .25rem;
        text-align: left;
        -webkit-appearance: none;
        -moz-appearance: none;
        appearance: none;
        position: relative;
    }
    .color-picker{
        position: relative;
        display: flex;
        width: 100%;
    }
    .vc-chrome,
    .vc-compact,
    .vc-sketch {
        position: absolute !important;
        /*bottom: 44px;*/
        top: 42px;
        z-index: 100;
    }
    .current-color {
        display: inline-block;
        width: 40px;
        height: 40px;
        background-color: #000;
        cursor: pointer;
    }
</style>
