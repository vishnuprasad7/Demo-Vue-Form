<template lang="pug">
  #app.section
    form-json(:formFields="jsonFields",
              :formName="'userProfil'")
</template>

<script>
import 'bulma/css/bulma.min.css'
import 'vue-form-json/dist/vue-form-json.css'
import formJson from 'vue-form-json'
import jsonFields from '@/assets/fields'

export default {
  name: 'app',
  components: {
    formJson
  },
  data: () => ({
    jsonFields
  }),
  mounted () {
    this.$root.$on('formSubmitted', values => alert(JSON.stringify(values)))
  }
}
</script>
