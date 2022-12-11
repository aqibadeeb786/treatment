<template>
  <div id="app">
    <!--Example dependecies-->
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/bulma/0.4.1/css/bulma.min.css"
    />

    <section class="section" style="padding-top: 0.5rem">
      <div class="container" v-if="!formSubmitted">
        <div class="columns">
          <div class="column is-8 is-offset-2">
            <horizontal-stepper
              :steps="demoSteps"
              @completed-step="completeStep"
              :top-buttons="true"
              @active-step="isStepActive"
              @stepper-finished="formSubmitted = true"
            >
            </horizontal-stepper>
          </div>
        </div>
      </div>
      <successView v-if="formSubmitted" />
      <button
        class="stepper-button add-app-btn"
        v-if="formSubmitted"
        @click="formSubmitted = false"
      >
        Add New Appointment
      </button>
    </section>
  </div>
</template>

<script>
import HorizontalStepper from "../src/HorizontalStepper.vue";
import AppointmentType from "./AppointmentType.vue";
import successView from "./successView.vue";
import StepOne from "./StepOne.vue";
import StepTwo from "./StepTwo.vue";
const teamUrl = "https://github.com/PygmySlowLoris";
const repoUrl = "https://github.com/PygmySlowLoris/vue-stepper";
export default {
  name: "app",
  components: {
    HorizontalStepper,
    successView,
  },
  data() {
    return {
      repoUrl: repoUrl,
      teamUrl: teamUrl,
      formSubmitted: false,
      demoSteps: [
        {
          icon: "mail",
          name: "first",
          title: "Appointment Selection",
          subtitle: "Appointment Details",
          component: AppointmentType,
          completed: false,
        },
        {
          icon: "report_problem",
          name: "PractitionerInformation",
          title: "Practitioner Appointment",
          subtitle: "Appointment Slots Selection",
          component: StepTwo,
          completed: false,
        },
        {
          icon: "report_problem",
          name: "CandidateInformation",
          title: "Candidate Information",
          subtitle: "Appointment Form",
          component: StepOne,
          completed: false,
        },
      ],
      activeStep: 0,
    };
  },
  computed: {},
  methods: {
    completeStep(payload) {
      this.demoSteps.forEach((step) => {
        if (step.name === payload.name) {
          step.completed = true;
        }
      });
    },
    isStepActive(payload) {
      this.demoSteps.forEach((step) => {
        if (step.name === payload.name) {
          if (step.completed === true) {
            step.completed = false;
          }
        }
      });
    },
    alert(payload) {
      console.log(payload)
    },
  },
};
</script>

<style scoped>
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.pointer {
  cursor: pointer;
}
h1,
h2 {
  font-weight: normal;
}
hr {
  background-color: transparent;
  border: none;
  display: block;
  height: inherit;
  margin: 1.5rem 0;
  border-top: dashed 1px;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #0b99b9;
  text-decoration: underline;
}
.text-medium-grey {
  color: #333;
}
.text-light-grey {
  color: #888;
}
.box.formated {
  position: relative;
  padding: 0;
}
.box.formated .heading {
  font-size: 1rem;
  text-transform: capitalize;
  padding: 0.8rem 1.5rem;
  background-color: #fafafa;
}
.box.formated .content {
  padding: 1rem 2rem;
}
i.top-left {
  position: absolute;
  left: 1.5rem;
  top: 0.8rem;
}
.vertical-separator {
  display: flex;
  justify-content: space-around;
}
.vertical-separator .line {
  border-right: 1px solid #cccccc;
}
.stepper-button {
  border: 2px solid #3383c8;
  color: #3383c8;
}
.add-app-btn{
  background-color: #3383c8;
  color: white;
  padding: 0.5rem 1rem;
  cursor: pointer;
  align-items: center;
  -webkit-box-pack: justify;
  -ms-flex-pack: justify;
  justify-content: space-between;
}
</style>
