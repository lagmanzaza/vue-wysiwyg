<template>
    <dropzone
        :id="_uid + 'vwdropzone'"
        :url="uploadURL"
        :useFontAwesome="true"
        @vdropzone-success="fileUploaded"
        @vdropzone-file-added="fileAdded"
        :autoProcessQueue="uploadURL !== 'None'"
        :dropzone-options="dropzoneOptions"
        :use-custom-dropzone-options="true"
        ref="dropzone"
        >
    </dropzone>
</template>

<script>
import dropzone from 'vue2-dropzone';
import bus from 'src/bus.js';

export default {
    name: "image",
    icon: '<svg width="1792" height="1792" viewBox="0 0 1792 1792" xmlns="http://www.w3.org/2000/svg"><path d="M576 576q0 80-56 136t-136 56-136-56-56-136 56-136 136-56 136 56 56 136zm1024 384v448h-1408v-192l320-320 160 160 512-512zm96-704h-1600q-13 0-22.5 9.5t-9.5 22.5v1216q0 13 9.5 22.5t22.5 9.5h1600q13 0 22.5-9.5t9.5-22.5v-1216q0-13-9.5-22.5t-22.5-9.5zm160 32v1216q0 66-47 113t-113 47h-1600q-66 0-113-47t-47-113v-1216q0-66 47-113t113-47h1600q66 0 113 47t47 113z"/></svg>',

    components: {
        dropzone
    },

    computed: {
        uploadURL () {
            return bus.options.image.uploadURL;
        },

        dropzoneOptions () {
          return bus.options.image.dropzoneOptions || {}
        }
    },

    methods: {
        fileUploaded (file, r) {
            const imagekey = bus.options.image.imageResKey;
            if (r)
                this.$emit("exec", "insertHTML", `<img src=${r[imagekey]}>`);
        },

        fileAdded (file) {
            // if no upload url is defined, insert image with base64 src
            if (file && this.uploadURL !== "None")
                return;

            let reader = new FileReader();

            reader.addEventListener("load", () => {
               this.$emit("exec", "insertHTML", `<img src=${reader.result}>`);
            }, false);

            reader.readAsDataURL(file);
        }
    },
}
</script>

