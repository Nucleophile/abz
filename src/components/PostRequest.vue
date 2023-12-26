<template>
  <article class="post-request" id="sign-up">
    <div class="container">
      <section v-if="!successfullyRegistered">
        <h1>Working with POST request</h1>
        <div class="content-small">
          <form method="POST" @submit.prevent="submitForm">
            <TextInput
              name="name"
              label="Your name"
              :error="getErrorPropValue('name')"
              @blur.once="validateLength(2, 60, $event)"
            />
            <TextInput
              type="email"
              name="email"
              label="Email"
              :error="getErrorPropValue('email')"
              @blur.once="validateRegEx"
            />
            <TextInput
              type="tel"
              name="phone"
              label="Phone"
              info="+38 (XXX) XXX - XX - XX"
              :error="getErrorPropValue('phone')"
              @blur.once="validateRegEx"
            />
            <fieldset class="position-fieldset">
              <legend class="position-fieldset__legend">
                Select your position
              </legend>
              <PreloaderEl v-if="loading" />
              <p v-if="loadingErrorMsg" class="error-msg">
                {{ loadingErrorMsg }}
              </p>
              <ul v-else>
                <li v-for="(position, i) in positions" :key="position.id">
                  <RadioInput
                    name="position_id"
                    :value="position.id"
                    :label="position.name"
                    :checked="i === 0 ? true : false"
                  />
                </li>
              </ul>
            </fieldset>
            <FileInput
              :error="getErrorPropValue('photo')"
              @change="validatePhoto"
            />
            <p class="error-msg form-error-msg" v-if="formSubmitErrorMsg">
              {{ formSubmitErrorMsg }}
            </p>
            <PreloaderEl class="form-preloader" v-if="sending" />
            <div class="ta-center">
              <button class="btn btn--default" :disabled="!isValid">
                Sign up
              </button>
            </div>
          </form>
        </div>
      </section>
      <section class="success-section" v-else>
        <h1>User successfully registered</h1>
        <img src="../assets/success-image.svg" alt="Successful registration" />
      </section>
    </div>
  </article>
</template>

<script>
import { ref, reactive, computed } from "vue";
import TextInput from "./TextInput.vue";
import RadioInput from "./RadioInput.vue";
import FileInput from "./FileInput.vue";
import PreloaderEl from "./PreloaderEl.vue";

export default {
  name: "PostRequest",
  emits: ["successful-registration"],
  setup(props, { emit }) {
    const positions = ref([]);
    const loading = ref(true);
    const sending = ref(false);
    const loadingErrorMsg = ref("");
    const fieldsValidity = reactive({
      name: null,
      email: null,
      phone: null,
      photo: null,
    });
    const validationRegExp = {
      email:
        /^(?:[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*|"(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21\x23-\x5b\x5d-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])*")@(?:(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?|\[(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?|[a-z0-9-]*[a-z0-9]:(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21-\x5a\x53-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])+)\])$/,
      phone: /^[\+]{0,1}380([0-9]{9})$/, // eslint-disable-line
    };
    const successfullyRegistered = ref(false);
    const formSubmitErrorMsg = ref("");
    const isValid = computed(() =>
      Object.values(fieldsValidity).every((fieldValidity) => fieldValidity)
    );

    // Get the positions list for radios
    fetch("https://frontend-test-assignment-api.abz.agency/api/v1/positions")
      .finally(() => (loading.value = false))
      .then((res) => res.json())
      .then((resJSON) => {
        if (resJSON.success) {
          positions.value = resJSON.positions;
        } else {
          throw new Error();
        }
      })
      .catch(() => {
        loadingErrorMsg.value =
          "Couldn't get positions list. Try to reload the page";
        fieldsValidity.position = false; // Not to allow form sending
      });

    // Validation of fields with input length rule
    function validateLength(minLength, maxLength, e, onInput) {
      let valid = false;

      if (
        e.target.value.length >= minLength &&
        e.target.value.length <= maxLength
      ) {
        valid = true;
      }

      fieldsValidity[e.target.name] = valid;

      if (!onInput) {
        // After first blur we start listening an input event
        e.target.addEventListener("input", (e) =>
          validateLength(minLength, maxLength, e, true)
        );
      }
    }

    // Validation of fields with input value correspondence to some regExp
    function validateRegEx(e, onInput) {
      const regexp = validationRegExp[e.target.name];
      fieldsValidity[e.target.name] = regexp.test(e.target.value);

      if (!onInput) {
        // After first blur we start listening an input event
        e.target.addEventListener("input", (e) => validateRegEx(e, true));
      }
    }

    // Validation of photo field
    function validatePhoto(e) {
      let imgWidth = 0;
      let imgHeight = 0;
      const img = document.createElement("img");
      const objectUrl = URL.createObjectURL(e.target.files[0]);

      img.onload = function () {
        imgWidth = this.width;
        imgHeight = this.height;
        URL.revokeObjectURL(objectUrl);

        if (
          e.target.files[0].type === "image/jpeg" &&
          e.target.files[0].size <= 5 * 1024 * 1024 &&
          imgWidth >= 70 &&
          imgHeight >= 70
        ) {
          fieldsValidity.photo = true;
        } else {
          fieldsValidity.photo = false;
        }
      };
      img.src = objectUrl;
    }

    // Not to show error message for empty fields after page loading
    function getErrorPropValue(field) {
      return fieldsValidity[field] === false ? true : false;
    }

    function submitForm(e) {
      if (!sending.value) { // To prevent several submitions
        sending.value = true;
        formSubmitErrorMsg.value = "";
        fetch("https://frontend-test-assignment-api.abz.agency/api/v1/token")
          .then((res) => res.json())
          .then((resJSON) => {
            if (resJSON.success) {
              const token = resJSON.token;
              return fetch(
                "https://frontend-test-assignment-api.abz.agency/api/v1/users",
                {
                  method: "POST",
                  headers: {
                    Token: token,
                  },
                  body: new FormData(e.target),
                }
              );
            } else {
              throw new Error();
            }
          })
          .finally(() => (sending.value = false))
          .then((res) => res.json())
          .then((resJSON) => {
            if (resJSON.success) {
              successfullyRegistered.value = true;
              emit("successful-registration");
            } else {
              throw new Error(resJSON.message);
            }
          })
          .catch(() => {
            formSubmitErrorMsg.value =
              "Sorry, something went wrong. Please, try again later";
          });
      }
    }

    return {
      positions,
      loading,
      sending,
      loadingErrorMsg,
      fieldsValidity,
      isValid,
      validateLength,
      validateRegEx,
      validatePhoto,
      getErrorPropValue,
      submitForm,
      successfullyRegistered,
      formSubmitErrorMsg,
    };
  },
  components: { TextInput, RadioInput, FileInput, PreloaderEl },
};
</script>

<style lang="scss">
.position-fieldset {
  border: none;
  margin-bottom: $form-control-mb - $radio-mb;
}
.position-fieldset__legend {
  margin-bottom: rem(12);
}
.success-section {
  text-align: center;
}
.error-msg {
  color: $error;
}
.form-error-msg,
.form-preloader {
  margin-bottom: $form-control-mb;
}
</style>
