<template>
  <div id="container">
    <h1>TimeRemain: {{ timeRemain }}</h1>
  </div>
</template>
<script>
  export default {
    name: 'Timer',

    data: function() {
      return {
        timeRemain: '' ,
      }
    },
    methods: {
      startCount: function(e) {
        this.timeRemain = e;
        let timer = setInterval(() => {
          this.timeRemain--
          if (this.timeRemain <= 0){
            this.timeRemain = 'Time up'
            clearInterval(timer);
            document.location = '/'
          }
        }, 1000)
      },
    },
  }

</script>
<style>

</style>