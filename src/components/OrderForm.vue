<template>
  <div class="form-container">
    <h1 class="heading">{{ title }}</h1>
    <b-form @submit.prevent="submit">
      <b-row class="mb-2">
        <b-form-group class="col-md-6 col-sm-12" label="Title">
          <b-form-input v-model="formItem.title" required></b-form-input>
        </b-form-group>
        <b-form-group class="col-md-6 col-sm-12" label="Type">
          <b-form-select v-model="formItem.type" required>
            <option value="raw">Raw Leads</option>
            <option value="qualified">Qualified Leads</option>
          </b-form-select>
        </b-form-group>
        <b-form-group class="col-md-6 col-sm-12" label="CPL">
          <b-form-input v-model="formItem.cpl" type="number" min="0" required></b-form-input>
        </b-form-group>
        <b-form-group class="col-md-6 col-sm-12" label="Max Devalidation (%)">
          <b-form-input v-model="formItem.max_devalidation" type="number" min="0" max="100" required></b-form-input>
        </b-form-group>
        <b-form-group class="col-md-6 col-sm-12" label="Priority">
          <b-form-input v-model="formItem.priority" type="number" required></b-form-input>
        </b-form-group>
        <b-form-group class="col-md-6 col-sm-12" label="Leads Max Age Raw (days)">
          <b-form-input v-model="formItem.leads_max_age_raw" type="number" min="0" required></b-form-input>
        </b-form-group>
        <b-form-group class="col-md-6 col-sm-12" label="Leads Max Age Qualified (days)">
          <b-form-input v-model="formItem.leads_max_age_qualified" type="number" min="0" required></b-form-input>
        </b-form-group>
        <b-form-group class="col-md-6 col-sm-12" label="Country">
          <b-form-input v-model="formItem.country" maxlength="2" required></b-form-input>
        </b-form-group>
        <b-form-group class="col-md-6 col-sm-12" label="Regions">
          <b-form-tags v-model="formItem.regions" separator=","></b-form-tags>
        </b-form-group>
        <b-form-group class="col-md-6 col-sm-12" label="Excluded Regions">
          <b-form-tags v-model="formItem.excluded_regions" separator=","></b-form-tags>
        </b-form-group>
        <b-form-group class="col-md-6 col-sm-12" label="Status">
          <b-form-select v-model="formItem.status" required>
            <option value="active">Active</option>
            <option value="stopped">Stopped</option>
          </b-form-select>
        </b-form-group>
        <b-form-group class="col-md-6 col-sm-12" label="ZIPs">
          <b-form-tags v-model="formItem.zips" separator=","></b-form-tags>
        </b-form-group>
        <b-form-group class="col-md-6 col-sm-12" label="Excluded ZIPs">
          <b-form-tags v-model="formItem.excluded_zips" separator=","></b-form-tags>
        </b-form-group>
        <b-form-group class="col-md-6 col-sm-12" label="Timezone">
          <b-form-input v-model="formItem.timezone" required></b-form-input>
        </b-form-group>
        <b-form-group class="col-md-6 col-sm-12" label="Starts At">
          <b-form-datepicker v-model="formItem.starts_at" required></b-form-datepicker>
        </b-form-group>
        <b-form-group class="col-md-6 col-sm-12" label="Ends At">
          <b-form-datepicker v-model="formItem.ends_at" required></b-form-datepicker>
        </b-form-group>
        <b-form-group class="col-md-6 col-sm-12" label="Excluded Dates">
          <b-form-tags v-model="formItem.excluded_dates" separator=","></b-form-tags>
        </b-form-group>
        <b-form-group class="col-md-6 col-sm-12" label="Monthly Cap">
          <b-form-input v-model="formItem.monthly_cap" type="number" min="-1" required></b-form-input>
        </b-form-group>
        <b-form-group class="col-md-6 col-sm-12" label="Total Cap">
          <b-form-input v-model="formItem.total_cap" type="number" min="-1" required></b-form-input>
        </b-form-group>
        <b-form-group class="col-md-6 col-sm-12" label="Delivery Type">
          <b-form-select v-model="formItem.delivery_type" required>
            <option value="api">API</option>
            <option value="email">Email</option>
          </b-form-select>
        </b-form-group>
        <b-form-group class="col-md-6 col-sm-12" label="Delivery Emails">
          <b-form-input v-model="formItem.delivery_emails" type="email" multiple></b-form-input>
        </b-form-group>
      </b-row>
      <hr>
      <b-row class="mb-2">
        <b-form-group class="col-12" label="Daily Caps">
          <b-form-group class="col-12">
            <b-row>
              <b-col v-for="(day, index) in daysOfWeek" :key="index" class="mb-2">
                <b-form-group :label="'' + day">
                  <b-form-input v-model="formItem['dc' + (index + 1)]" type="number" min="0"></b-form-input>
                </b-form-group>
              </b-col>
            </b-row>
          </b-form-group>
        </b-form-group>
      </b-row>
      <b-row class="mb-2">
        <b-form-group class="col-6">
          <b-button type="submit" variant="primary">Save</b-button>
        </b-form-group>
      </b-row>
    </b-form>
  </div>
</template>

<script>
import orders from '@/api/orders'
export default {
  name: 'OrderForm',
  data () {
    return {
      loading: true,
      formItem: {},
      daysOfWeek: ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
    }
  },
  computed: {
    title () {
      return this.loading ? 'Loading...' : this.formItem.title;
    }
  },
  created () {
    this.formItem = orders.getOrder(1);
    this.loading = false;
  },
  methods: {
    submit () {
      // Add form validation logic here
      if (this.validateForm()) {
        alert('Form submitted successfully');
      } else {
        alert('Please correct the errors in the form');
      }
    },
    validateForm () {
      // Implement form validation logic here
      let isValid = true;
      // Example validation
      if (!this.formItem.title || !this.formItem.type || !this.formItem.cpl) {
        isValid = false;
      }
      return isValid;
    }
  }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&display=swap');

.heading {
    font-family: 'Poppins', sans-serif;
    font-size: 50px;
    font-weight: 600;
}
.form-container {
  position: relative;
  padding: 20px;
  border-radius: 8px;
  background-image: url(http://localhost:8080/img/bg.f071b7d3.jpg), linear-gradient(154deg, rgb(243 243 243), rgb(254 214 227 / 94%));

  background-size: cover;
  background-blend-mode: overlay; /* Blends the gradient with the image */
  overflow: hidden;
  margin-top: 20px;
  /* filter: blur(10px);  */
}


.b-form {
  position: relative;
  z-index: 1;
}
button{
  border: none !important;
  outline: none !important;
}
input{
  border-radius:20px;
}
select{
  width: 98%;
    padding: 6px;
    border: 1px solid #dee2e6;
    border-radius: 20px;
}
.b-form-datepicker{
    width: 98%;
    border: 1px solid #dee2e6;
    border-radius: 20px;
}
input:focus{
  box-shadow: none;
}
select:focus{
  box-shadow: none;
}
.b-form-datepicker:focus{
box-shadow: none;
}
.form-control:focus{
  box-shadow: none !important;
}
</style>
