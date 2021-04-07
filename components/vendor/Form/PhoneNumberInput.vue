<template>
  <form-group LabelFor="phone" label="Numéro de téléphone">
    <vue-tel-input :onlyCountries="['CI', 'GN', 'SN', 'BJ']" @validate="onValidate"  @input="onInput" validCharactersOnly></vue-tel-input>
  </form-group>
</template>

<script>
  export default {
    data () {
      return {
        phone : ''
      }
    },
    methods : {
      onValidate({number, isValid, country}) {
        console.log(isValid)
      },

      onInput (e) {
        console.log(e)
      },
      CountryChanged(val) {
        console.log(val)
      }
    }
  }
</script>
