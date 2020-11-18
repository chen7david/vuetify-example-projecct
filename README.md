# vuetify-example-projecct

### Lazy Image Loader
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
            >
                <slot name="surface"></slot>
                <template v-slot:placeholder>
                    <v-row class="fill-height ma-0" align="center" justify="center">
                        <v-progress-circular
                            indeterminate
                            color="red"
                        ></v-progress-circular>
                    </v-row>
                </template>
            </v-img>
        </v-sheet>
    </v-lazy>
</template>
```

```vue
<script>
export default {
    name: 'lazyImg',
    props: {
        width: null,
        aspect: null,
        src: null,
        onImgError: null,
        onDone: null
    },
    data: () => ({
        isActive: null
    }),
    computed: {
        height: (props) => {
            const [w,h] = props.aspect.split('/') 
            return (props.width/w) * h
        },
    }
}
</script>
```