<template>
    <div :class="isSelected ? 'selected frame' : 'nonSelected frame'" @mouseleave="onImageLeave" @click="onImageClick">
        <div class="image">
            <v-card>
                <v-img 
                    :src="imageData" 
                    aspect-ratio="1"
                    >
                </v-img>
            </v-card>
            <v-btn v-if="isSelected" fab dark small class="btn" color="gray" @click="onDelete">
                <v-icon>fa-times</v-icon>
            </v-btn>
        </div>
    </div>
</template>

<script>
export default {
    name: 'AddImage',
    props: {
        image: {
            type: Object,
            default: null
        },
    },

    data() {
        return {
            imageData: "",
            isSelected: false
        };
    },
    computed: {
        selected() {
            return this.selectedLayout === this.$options.name && !this.used;
        },
    },
    mounted() {
        this.setImage(this.image);
    },
    watched: {
    },
    methods: {
        setImage(image) {
            if(!image.contentType) return;
            this.imageData = image.contentType + "," + image.data;
        },
        onImageClick() {
            this.isSelected = !this.isSelected;
        },
        onImageLeave() {
            this.isSelected = false;
        },
        onDelete() {
            this.$emit("onImageDelete", this.image.id);
        },
        onLoadSuccess() {
            this.$emit("onLoadSuccess");
        },
        onLoadError() {
            this.$emit("onLoadError");
        },
    },
    watch: {
    },
}
</script>
<style scoped>
.selected {
  background: linear-gradient(to bottom, #3acfd5 0%, #11c3f0 100%);
  position: relative;
  cursor: pointer;
  border-radius: 5px 5px 5px 5px / 5px 5px 5px 5px;
}
.nonSelected {
  position: relative;
  cursor: pointer;
  border-radius: 5px 5px 5px 5px / 5px 5px 5px 5px;
}

.frame:after {
  content: "";
  display: block;
  padding-bottom: 141% !important;
}
.frame div {
  position: absolute;
  width: 100%;
  height: 100%;
}
.frame div > img {
  object-fit: contain;
}

.image {
  position: relative;
  height: calc(100% - 8px) !important;
  width: calc(100% - 8px) !important;
  margin-top: 4px;
  margin-left: 4px;
}
.btn {
  position: absolute;
  top: 10px;
  right: 5px;
  z-index: 2;
}
</style>
