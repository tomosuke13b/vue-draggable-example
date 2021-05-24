<template>
  <v-container>
    <v-row>
        <v-col cols="12">
          <v-col>
            <input
                type="file"
                accept="image/png,image/jpeg, image/jpg"
                @change="addImages($event)"
                style="display: none;"
                ref="input"
                :multiple="true"
            />
          </v-col>
        </v-col>
        <Draggable class="row" v-model="images" element="div" :move="onMove" @end="onDragEnd">
          <v-col
              cols="3"
              v-for="(image, index) in images"
              :key="index">
              <AddControl v-if="image.isControl" @onImageAdd="inputFiles" />
              <AddImage v-else :image="image" @onImageDelete="onImageDelete" class="page" />
          </v-col>
        </Draggable>
    </v-row>
  </v-container>
</template>

<script>
  import Draggable from 'vuedraggable'
  import { AddImage, AddControl } from "./item";
  export default {
    name: 'Sample',

    components: {
      Draggable,
      AddImage,
      AddControl
    },

    data: () => ({
      images: [],
    }),
    mounted() {
      this.addControlImage(this.images);
    },
    methods: {
        inputFiles() {
            this.$refs.input.click();
        },
        refresh(images) {
            var oldImages = images;
            setTimeout(
                () => {
                    var newImages = [];
                    for (const index in oldImages ) {
                        var oldImage = oldImages[index];
                        if (oldImage.isControl) continue;
                        newImages.push(oldImage);
                    }
                    this.addControlImage(newImages);
                    this.images = newImages;
                },1
            );
            this.images = [];
        },
        async addImages(event) {
            let target = event.target;
            let files = target.files;

            await this.readFiles(files);
            this.refresh(this.images);
            this.$refs.input.value = "";
        },
        async readFiles(files) {
            for (const index in files ) {
                var file = files[index];
                if ((typeof file) !== "object") continue;
                await this.addImage(file);
            }
        },
        async addImage(file) {
            let result = await this.readAsDataURL(file);
            let imageSource = result.split(',');
            let contentType = imageSource[0];
            let data = imageSource[1];
            this.images.push(
              {
                  id: this.images.length,
                  name: file.name,
                  contentType: contentType,
                  data: data,
                  isControl: false
              }
            );
        },
        readAsDataURL(blob) {
            return new Promise((resolve, reject) => {
                let reader = new FileReader();
                reader.onload = () => { resolve(reader.result); };
                reader.onerror = () => { reject(reader.error); };
                reader.readAsDataURL(blob);
            });
        },
        addControlImage(images) {
            images.push(
                {
                  name: "Control",
                  isControl: true
                }
            );
        },
        onMove({ relatedContext, draggedContext }) {
            const relatedElement = relatedContext.element;
            const draggedElement = draggedContext.element;
            return (
                (!relatedElement || !relatedElement.isControl) && !draggedElement.isControl
            );
        },
        onImageDelete(id) {
          var newImages = this.images.filter((image) => { image.id !== id && !image.isControl });
          setTimeout(
              () => {
                  this.addControlImage(newImages);
                  this.images = newImages;
              },1
          );
          this.images = [];
        },
        onDragEnd(e) {
          if (e.newIndex === e.oldIndex) return;
          var images = this.images;
          var newImage = images[e.newIndex];
          var oldImage = images[e.oldIndex];
          images[e.newIndex] = newImage;
          images[e.oldIndex] = oldImage;
          this.refresh(images);
        },
    },
  }
</script>
