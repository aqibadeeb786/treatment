<template>
  <div style="padding: 2rem 3rem; text-align: left">
    <div class="field">
      <label class="label">Sex :</label>
      <div class="control">
        <input
          :class="['input', $v.form.sex.$error ? 'is-danger' : '']"
          type="text"
          v-model="form.sex"
        />
      </div>
      <p v-if="$v.form.sex.$error" class="help is-danger">This sex invalid</p>
    </div>
    <div class="field">
      <label class="label">First Name :</label>
      <div class="control">
        <input
          :class="['input', $v.form.firstName.$error ? 'is-danger' : '']"
          type="text"
          v-model="form.firstName"
        />
      </div>
      <p v-if="$v.form.firstName.$error" class="help is-danger">
        This first name is invalid
      </p>
    </div>
    <div class="field">
      <label class="label">Family Name :</label>
      <div class="control">
        <input
          :class="['input', $v.form.familyName.$error ? 'is-danger' : '']"
          type="text"
          v-model="form.familyName"
        />
      </div>
      <p v-if="$v.form.familyName.$error" class="help is-danger">
        This family name is invalid
      </p>
    </div>
    <div class="field">
      <label class="label">Date of birth :</label>
      <div class="control">
        <input
          :class="['input', $v.form.dateOfBirth.$error ? 'is-danger' : '']"
          type="date"
          v-model="form.dateOfBirth"
        />
      </div>
      <p v-if="$v.form.dateOfBirth.$error" class="help is-danger">
        This date of birth is invalid
      </p>
    </div>
    <div class="field">
      <label class="label">Email :</label>
      <div class="control">
        <input
          :class="['input', $v.form.email.$error ? 'is-danger' : '']"
          type="email"
          v-model="form.email"
        />
      </div>
      <p v-if="$v.form.email.$error" class="help is-danger">
        This email is invalid
      </p>
    </div>
  </div>
</template>

<script>
import { validationMixin } from "vuelidate";
import { required, email } from "vuelidate/lib/validators";
export default {
  props: ["clickedNext", "currentStep"],
  mixins: [validationMixin],
  data() {
    return {
      form: {
        dateOfBirth: "",
        familyName: "",
        sex: "",
        firstName: "",
        email: "",
      },
    };
  },
  validations: {
    form: {
      firstName: {
        required,
      },
      email: {
        required,
        email,
      },
      sex: {
        required,
      },
      dateOfBirth: {
        required,
      },
      familyName: {
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