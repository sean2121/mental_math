<template>
  <div id="container">
    <h1>TEST RESULT IS HERE</h1>
    <p>{{ scores }}</p>
  </div>
</template>
<script>
  export default {
    name: 'TrainingResult',
    props: ['scores'],
    data: function() {
      return {
        result: '' ,
      }
    },
    methods: {
    }
  }

</script>
<style>

</style>