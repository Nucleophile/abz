<template>
  <div class="form-control" :class="{ 'form-control--error': error }">
    <input
      class="text-input"
      :type="type"
      :id="id"
      :name="name"
      placeholder=" "
      @blur.once="validate"
      @input="validate"
    />
    <label class="text-input-label" :for="id">{{ label }}</label>
    <div class="text-input-info text-input-info--error" v-if="error === true">
      Wrong value
    </div>
    <div class="text-input-info" v-if="infoMsg">{{ infoMsg }}</div>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  name: "TextInput",
  props: {
    type: {
      type: String,
      default: "text",
    },
    name: String,
    id: {
      type: String,
      default: (props) => props.name,
    },
    label: String,
    info: String,
    error: Boolean,
  },
  emits: ["validate"],
  setup(props, { emit }) {
    let infoMsg = ref(props.info);
    let blured = false;

    function validate(e) {
      if (e.type === "input" && !blured) {
        return;
      } else {
        blured = true;
      }
      emit("validate", e);
    }

    return { infoMsg, validate };
  },
  components: {},
};
</script>

<style lang="scss">
.form-control {
  position: relative;
  margin-bottom: $form-control-mb;
  &--error {
    .text-input,
    .file-input,
    .file-input-label__button,
    .file-input-label__text {
      border-color: $error;
    }
  }
}
.text-input,
.file-input,
.file-input-label__button,
.file-input-label__text {
  padding: $input-padding;
  border: $input-border solid $input-border-color;
}
.text-input,
.file-input,
.file-input-label {
  display: block;
  width: 100%;
  @include defaultFont;
  background: $page-bg;
  border-radius: $input-border-radius;
  color: #000;
}
.text-input {
  &:focus,
  &:not(:placeholder-shown) {
    + .text-input-label {
      @include inputInfoFont;
      top: math.div(($input-label-focused-lh + $input-label-padding), 2) * (-1);
    }
  }
}
.text-input-label {
  color: $input-label-color;
  position: absolute;
  top: $input-padding + $input-border;
  left: $input-padding + $input-border;
  padding: $input-label-padding;
  margin: -$input-label-padding 0 0 (-$input-label-padding);
  background: $page-bg;
  transition-property: top, font-size, line-height;
  transition-duration: 0.2s;
}
.text-input-info {
  @include inputInfoFont;
  color: $input-info-color;
  margin: rem(4) 0 0 rem(16);
  &--error {
    color: $error;
  }
}
</style>
