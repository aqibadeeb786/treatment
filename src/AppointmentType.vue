<template>
  <div style="padding: 2rem 3rem; text-align: left">
    <div class="field">
      <label class="label">Treatment Name :</label>
      <div class="control">
        <input
          :class="['input', $v.form.name.$error ? 'is-danger' : '']"
          type="text"
          v-model="form.name"
        />
      </div>
      <p v-if="$v.form.name.$error" class="help is-danger">
        This treatment name invalid
      </p>
    </div>
    <div class="field">
      <label class="label">Treatment Description :</label>
      <div class="control">
        <input
          :class="['input', $v.form.description.$error ? 'is-danger' : '']"
          type="text"
          v-model="form.description"
        />
      </div>
      <p v-if="$v.form.description.$error" class="help is-danger">
        This treatment description is invalid
      </p>
    </div>
    <div class="field">
      <label class="label">Duration : {{ form.duration }}</label>
    </div>
  </div>
</template>

<script>
import { validationMixin } from "vuelidate";
import { required } from "vuelidate/lib/validators";
export default {
  props: ["clickedNext", "currentStep"],
  mixins: [validationMixin],
  data() {
    return {
      form: {
        name: "",
        description: "",
        duration: "30 Minutes",
      },
    };
  },
  validations: {
    form: {
      name: {
        required,
      },
      description: {
        required,
      },
    },
  },
  watch: {
    $v: {
      handler: function (val) {
        if (!val.$invalid) {
          this.$emit("can-continue", { value: true });
        } else {
          this.$emit("can-continue", { value: false });
          setTimeout(() => {
            this.$emit("change-next", { nextBtnValue: false });
          }, 3000);
        }
      },
      deep: true,
    },
    clickedNext(val) {
      console.log(val);
      if (val === true) {
        this.$v.form.$touch();
      }
    },
  },
  mounted() {
    if (!this.$v.$invalid) {
      this.$emit("can-continue", { value: true });
    } else {
      this.$emit("can-continue", { value: false });
    }
  },
};
</script>