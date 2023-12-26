<template>
  <div class="form-control radio-input-form-control">
    <input
      class="radio-input"
      type="radio"
      :name="name"
      :value="value"
      :id="id"
      :checked="checked"
    />
    <label class="radio-input-label" :for="id">{{ label }}</label>
  </div>
</template>

<script>
export default {
  name: "RadioInput",
  props: {
    name: String,
    value: Number,
    label: String,
    checked: Boolean,
  },
  setup(props) {
    const id = props.name + props.value;

    return { id };
  },
};
</script>

<style lang="scss">
.radio-input-form-control {
  margin-bottom: $radio-mb;
}
.radio-input,
.radio-input-label::before {
  width: $radio-size;
  height: $radio-size;
  position: absolute;
  left: 0;
  top: math.div($default-font-lh - $radio-size, 2);
}
.radio-input {
  appearance: none;
  &-label {
    padding-left: rem(32);
    &::before {
      content: "";
      display: block;
      border: $input-border solid $input-border-color;
      border-radius: $radio-size;
      transition: border-color 0.2s;
    }
    &::after {
      content: "";
      display: block;
      width: math.div($radio-size, 2);
      height: math.div($radio-size, 2);
      border-radius: math.div($radio-size, 2);
      position: absolute;
      top: math.div($default-font-lh - $radio-size, 2) +
        math.div($radio-size, 4);
      left: math.div($radio-size, 4);
      background: $secondary;
      transform: scale(0);
      transition: transform 0.2s;
    }
  }
  &:checked {
    + .radio-input-label {
      &::before {
        border-color: $secondary;
      }
      &::after {
        transform: scale(1);
      }
    }
  }
}
</style>
