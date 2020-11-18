# vuetify-example-projecct

### Lazy Image Loader

#### Usage
```vue
<template>
    <v-hover v-slot:default="{ hover }" open-delay="10">
        <v-lazy-img
            :src="$posterURL"
            width="130"
            aspect="2/3"
            :onDone="onDone"
            spinnerColor="orange"
        >
            <template v-slot:surface>
                <v-overlay absolute :value="hover">
                    <v-btn @click="somefunction" x-large icon>
                        <v-icon>mdi-play</v-icon>
                    </v-btn>
                </v-overlay>
            </template>

        </v-lazy-img>
    </v-hover>
</template>

<script>
import lazyImg from 'lazyImg'
export default {
    name: 'SomeComponent',
    components: {
        'v-lazy-img': lazyImg
    }
}
</script>
```


```vue
<template>
    <v-lazy
        v-model="isActive"
        :options="{threshold: 0.5}"
        :min-height="height"
        transition="fade-transition"
    >
        <v-sheet
            :width="width"
            :height="height"
        >
            <v-img
                :src="src" 
                :height="height"
                :aspect-ratio="aspect"
                @error="errorHandler"
                @load="doneHandler"
            >
                <slot name="surface"></slot>
                <template v-slot:placeholder>
                    <v-row class="fill-height ma-0" align="center" justify="center">
                        <v-progress-circular
                            v-if="!failed"
                            indeterminate
                            :color="spinnerColor || `red`"
                        ></v-progress-circular>
                    </v-row>
                </template>
            </v-img>
        </v-sheet>
    </v-lazy>
</template>
```
#### Implementation

```vue
<script>
export default {
    name: 'lazyImg',
    props: {
        width: null,
        aspect: null,
        src: null,
        onError: null,
        onDone: null,
        spinnerColor: null
    },
    data: () => ({
        isActive: null,
        failed: false,
        done: false,
    }),
    computed: {
        height: (props) => {
            const [w,h] = props.aspect.split('/') 
            return (props.width/w) * h
        },
    },
    methods: {
        errorHandler(){
            this.failed = true
            if(this.onError) this.onError() 
        },
        doneHandler(){
            this.done = true
            if(this.onDone) this.onDone() 
        }
    }
}
</script>
```