<template>
  <div class="form-control" :class="{ 'form-control--error': error }">
    <input
      class="file-input"
      type="file"
      name="photo"
      id="photo"
      accept="image/jpg, image/jpeg"
      @change="inputChange"
      v-bind="$attrs"
    />
    <label
      class="file-input-label"
      :class="{ 'file-input-label--selected': fileSelected }"
      for="photo"
    >
      <span class="file-input-label__button">Upload</span>
      <span class="file-input-label__text">{{ labelText }}</span>
    </label>
    <div class="text-input-info text-input-info--error" v-if="error === true">
      Wrong value. Minimum size of photo is 70x70px. The photo format must be jpeg/jpg. The photo size must not be greater than 5&nbsp;Mb
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  name: "FileInput",
  inheritAttrs: false,
  props: {
    error: Boolean
  },
  setup() {
    const labelTextDefault = "Upload your photo";
    const labelText = ref(labelTextDefault);
    const fileSelected = ref(false);

    function inputChange(e) {
      if (e.target.files[0]) {
        labelText.value = e.target.files[0].name;
        fileSelected.value = true;
      } else {
        // In webkit browsers canceling new file chosing is deleting already chosen file
        labelText.value = labelTextDefault;
        fileSelected.value = false;
      }
    }

    return { labelText, fileSelected, inputChange };
  },
};
</script>

<style lang="scss">
.file-input-label {
  display: flex;
  position: absolute;
  top: 0;
  left: 0;
  &--selected {
    .file-input-label__text {
      color: #000;
    }
  }
  &__button {
    border-color: #000;
    border-top-left-radius: $input-border-radius;
    border-bottom-left-radius: $input-border-radius;
  }
  &__text {
    flex-grow: 1;
    border-left: none;
    border-top-right-radius: $input-border-radius;
    border-bottom-right-radius: $input-border-radius;
    color: $input-label-color;
  }
}
</style>
