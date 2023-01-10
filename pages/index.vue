<template>
  <b-container>
    <b-form @submit="submitForm">
      <h1>Report a Bug</h1>
      <h3>Basic Questions:</h3>
      <b-form-group>
        <label>URL of Page</label>
        <b-form-input 
          placeholder="'code.goodmeasures.com/members'"
          type="text"
          v-model="form.url"
          :state="validateState('url')"
          aria-describedby="url-input-live-feedback"
        ></b-form-input>

        <b-form-invalid-feedback id="url-input-live-feedback">This field is required.</b-form-invalid-feedback>
      </b-form-group>

      <b-form-group>
        <label>Summary</label>
        <b-form-input
          type="text"
          v-model="form.summary"
          :state="validateState('summary')"
          aria-describedby="summary-input-live-feedback"
        ></b-form-input>

        <b-form-invalid-feedback id="summary-input-live-feedback">Must be at least 20 characters long.</b-form-invalid-feedback>
      </b-form-group>

      <b-form-group>
        <label>Description</label>
        <b-form-input
          type="text"
          v-model="form.description"
          :state="validateState('description')"
          aria-describedby="description-input-live-feedback"
        ></b-form-input>

        <b-form-invalid-feedback id="description-input-live-feedback">Must be at least 35 characters long.</b-form-invalid-feedback>
      </b-form-group>

      <h3>Qualifying Questions:</h3>

      <b-form-group>
        <label>Does the website still work for patients and the team even WITH this issue?</label>
        <b-form-radio
          v-model="form.elevateSeverity1"
          :value="false"
          :state="validateState('elevateSeverity1')"
        >
          Yes
        </b-form-radio>
        <b-form-radio
          v-model="form.elevateSeverity1"
          :value="true"
          :state="validateState('elevateSeverity1')"
        >
          No
        </b-form-radio>
        <b-form-invalid-feedback :state="validateState('elevateSeverity1')">This field is required. Please select an option.</b-form-invalid-feedback>
      </b-form-group>
      
      <b-form-group>
        <label>Is this issue preventing you from doing your daily tasks?</label>
        <b-form-radio
          v-model="form.elevateSeverity2"
          :value="true"
          :state="validateState('elevateSeverity2')"
        >
          Yes
        </b-form-radio>
        <b-form-radio
          v-model="form.elevateSeverity2"
          :value="false"
          :state="validateState('elevateSeverity2')"
        >
          No
        </b-form-radio>
        <b-form-invalid-feedback :state="validateState('elevateSeverity2')">This field is required. Please select an option.</b-form-invalid-feedback>
      </b-form-group>
      
      <b-form-group>
        <label>Does this issue increase the amount of time needed to complete tasks?</label>
        <b-form-radio
          v-model="form.elevateSeverity3"
          :value="true"
          :state="validateState('elevateSeverity3')"
        >
          Yes
        </b-form-radio>
        <b-form-radio
          v-model="form.elevateSeverity3"
          :value="false"
          :state="validateState('elevateSeverity3')"
        >
          No
        </b-form-radio>
        <b-form-invalid-feedback :state="validateState('elevateSeverity3')">This field is required. Please select an option.</b-form-invalid-feedback>
      </b-form-group>

      <b-button variant="secondary">Cancel</b-button>
      <b-button type="submit" variant="success">Submit</b-button>
    </b-form>
  </b-container>
</template>

<script>
import { validationMixin } from "vuelidate";
import { required, minLength } from "vuelidate/lib/validators";

export default {
  name: 'IndexPage',
  mixins: [validationMixin],
  data() {
    return {
      severityLevel: 'Low',
      form: {
        url: '',
        summary: '',
        description: '',
        elevateSeverity1: null,
        elevateSeverity2: null,
        elevateSeverity3: null
      }
    }
  },
  validations: {
    form: {
      url: {
        required,
      },
      summary: {
        required,
        minLength: minLength(20)
      },
      description: {
        required,
        minLength: minLength(35)
      },
      elevateSeverity1: {
        required
      },
      elevateSeverity2: {
        required
      },
      elevateSeverity3: {
        required
      }
    }
  },
  methods: {
    calculateSeverityLevel() {
      if (this.elevateSeverity1 === "true" ||  (this.elevateSeverity2 === "true" && this.elevateSeverity3 === "true")) {
        this.severityLevel = "Highest"
      } else if (this.elevateSeverity1 === "false" && this.elevateSeverity2 === "true" && this.elevateSeverity3 === "true") {
        this.severityLevel = "High"
      } else if (this.elevateSeverity1 === "false" && this.elevateSeverity2 === "false" && this.elevateSeverity3 === "true") {
        this.severityLevel = "Medium"
      }
    },
    submitForm(event) {
      event.preventDefault()
      this.$v.form.$touch();
      if (this.$v.form.$anyError) {
        return;
      } else {
        this.calculateSeverityLevel()
        this.$router.push('/success')
      }
      // POST to JIRA goes here eventually
    },
    validateState(input) {
      const { $dirty, $error } = this.$v.form[input];
      return $dirty ? !$error : null;
    },
  }
}
</script>
