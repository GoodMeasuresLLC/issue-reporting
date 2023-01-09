<template>
  <b-container>
    <b-form @submit="submitForm">
      <h1>Issue Reporting:</h1>
      <h3>Basic Questions:</h3>
      <b-form-group>
        <label>URL of Page</label>
        <b-form-input placeholder="'code.goodmeasures.com/members'" type="text"></b-form-input>
      </b-form-group>
      <b-form-group>
        <label>Summary</label>
        <b-form-input type="text"></b-form-input>
      </b-form-group>
      <b-form-group>
        <label>Description</label>
        <b-form-input type="text"></b-form-input>
      </b-form-group>

      <h3>Qualifying Questions:</h3>
      <b-form-group>
        <label>Does the website still work for patients and the team even WITH this issue?</label>
        <b-form-radio v-model="elevateSeverity1" value="false">Yes</b-form-radio>
        <b-form-radio v-model="elevateSeverity1" value="true">No</b-form-radio>
      </b-form-group>
      <b-form-group>
        <label>Is this issue preventing you from doing your daily tasks?</label>
        <b-form-radio v-model="elevateSeverity2" value="true">Yes</b-form-radio>
        <b-form-radio v-model="elevateSeverity2" value="false">No</b-form-radio>
      </b-form-group>
      <b-form-group>
        <label>Does this issue increase the amount of time needed to complete tasks?</label>
        <b-form-radio v-model="selectedQualifier3" value="true">Yes</b-form-radio>
        <b-form-radio v-model="selectedQualifier3" value="false">No</b-form-radio>
      </b-form-group>

      <b-button variant="secondary">Cancel</b-button>
      <b-button type="submit" variant="success">Submit</b-button>
    </b-form>
  </b-container>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      elevateSeverity1: '',
      elevateSeverity2: '',
      selectedQualifier3: '',
      severityLevel: 'Low'
    }
  },
  methods: {
    calculateSeverityLevel() {
      if (this.elevateSeverity1 === "true" ||  (this.elevateSeverity2 === "true" && this.elevateSeverity3 === "true")) {
        this.severityLevel = "Highest"
      } else if (this.elevateSeverity1 === "false" && this.elevateSeverity2 === "true" && this.selectedQualifier3 === "true") {
        this.severityLevel = "High"
      } else if (this.elevateSeverity1 === "false" && this.elevateSeverity2 === "false" && this.selectedQualifier3 === "true") {
        this.severityLevel = "Medium"
      }
    },
    submitForm(event) {
      event.preventDefault()
      this.calculateSeverityLevel()
      // POST to JIRA goes here eventually
      this.$router.push('/success')
    }
  }
}
</script>
