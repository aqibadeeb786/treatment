<template>
  <div class="card" style="margin: 3rem">
    <div class="card-content" align="left">
      <div class="field">
        <label class="label">Add appointment date : </label>
        <div class="control">
          <input
            :class="['input', $v.form.date.$error ? 'is-danger' : '']"
            type="date"
            v-model="form.date"
          />
        </div>
        <p v-if="$v.form.date.$error" class="help is-danger">
          This date invalid
        </p>
      </div>
      <div class="field">
        <label class="label">appointment time Slot :</label>
        <div class="control">
          <input
            :class="['input', $v.form.timeSlot.$error ? 'is-danger' : '']"
            type="time"
            v-model="form.timeSlot"
          />
        </div>
        <p v-if="$v.form.timeSlot.$error" class="help is-danger">
          This time slot invalid
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import { validationMixin } from "vuelidate";
import { required } from "vuelidate/lib/validators";
export default {
  props: ["currentStep"],
  mixins: [validationMixin],
  data() {
    return {
      form: {
        date: "",
        timeSlot: "",
      },
    };
  },
  validations: {
    form: {
      date: {
        required,
      },
      timeSlot: {
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
  methods: {
    canContinue() {
      this.$emit("can-continue", { value: true });
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