<template lang="html">
    <div class="section">
    <div class="container">

    <form class="panel" @submit.prevent="submitForm">
      <p class="panel-heading">
        Sign Up For My Web App
      </p>


    <div class="panel-block">
      <p class="control has-icon" v-for= "input in formInputs">
      <template v-if="input.type === 'select'" v-model="formValues[input.id]">
        <span class="select is-fullwidth">
    <select>
        <option v-for="option in input.options"> {{ option.label }} </option>
    </select>
  </span>
      </template>

      <template v-else>
        <template v-if="input.type === 'textarea'">
          <textarea class="input textarea" :placeholder="input.label" v-model="formValues[input.id]"></textarea>
        <i :class="input.icon" class="fa" aria-hidden="true"></i>
        </template>

      <template v-else>
        <input class="input" :placeholder="input.label" v-model="formValues[input.id]">
        <i :class="input.icon" class="fa" aria-hidden="true"></i>
      </template>
    </template>
  </p>
</div>

  <div class="panel-block">
     <button class="button is-fullwidth is-primary"> Submit </button>
  </div>
</form>
 </div>
 </div>
</template>



<script>
const apiUrl = 'http://json-data.herokuapp.com/forms';


export default {
  data() {
    return {
      apiUrl: apiUrl,
      formInputs: [],
      formValues: {},
    };
  },

  mounted() {
    this.getData();
  },

  methods: {
    getData() {
      fetch(`${apiUrl}`)
        .then((r) => r.json())
        .then((formInputs) => {
          this.formInputs = formInputs;
        });
    },

    submitForm(formValues) {
      console.log(this.formValues);
      fetch ('http://tiny-tn.herokuapp.com/collections/form-stephanie', {
        method: 'POST',
        headers: {'content-type': 'application/json',},
        body: JSON.stringify(this.formValues),
      })
      .then(() => {
        this.formValues = {};
      });
  },
},
};
</script>
