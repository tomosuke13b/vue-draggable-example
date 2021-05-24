<template>
    <div class="card">
        <v-img :src="imageData" aspect-ratio="1"  @click="onClick"></v-img>
    </div>
</template>

<script>
import settings from "@/settings.js";

export default {
    name: 'AddControl',
    props: {
    },

    data() {
        return {
            imageData: settings.IMAGE_ADD
        };
    },
    computed: {
    },
    created() {
    },
    mounted() {
        this.$el.addEventListener('pointerdown', this.pointerdown);
        this.$el.addEventListener('touchmove', this.touchmove);
        this.$el.addEventListener('drag', this.drag);
        this.$el.addEventListener('dragend', this.dragend);
        this.$el.addEventListener('dragenter', this.dragenter);
        this.$el.addEventListener('dragover', this.dragover);
        this.$el.addEventListener('dragleave', this.dragleave);
        this.$el.addEventListener('drop', this.drop);
    },
    destroyed() {
        this.$el.removeEventListener('pointerdown', this.pointerdown);
        this.$el.removeEventListener('touchmove', this.touchmove);
        this.$el.removeEventListener('drag', this.drag);
        this.$el.removeEventListener('dragend', this.dragend);
        this.$el.removeEventListener('dragenter', this.dragenter);
        this.$el.removeEventListener('dragover', this.dragover);
        this.$el.removeEventListener('dragleave', this.dragleave);
        this.$el.removeEventListener('drop', this.drop);
    },
    watched: {
    },
    methods: {
        pointerdown(event) {
            event.stopImmediatePropagation();
        },
        touchmove(event) {
            event.stopImmediatePropagation();
        },
        drag(event) {
            event.stopImmediatePropagation();
        },
        dragend(event) {
            event.stopImmediatePropagation();
        },
        dragenter(event) {
            event.stopImmediatePropagation();
        },
        dragover(event) {
            event.stopImmediatePropagation();
        },
        dragleave(event) {
            event.stopImmediatePropagation();
        },
        drop(event) {
            event.stopImmediatePropagation();
        },
        onClick() {
            this.$emit("onImageAdd");
        },
    },
    watch: {
    },
}
</script>
<style scoped>
.card {
    border-style: dashed;
}
</style>
