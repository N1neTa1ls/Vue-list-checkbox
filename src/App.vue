<template>
  <div id="app">
    <Field />
  </div>
</template>

<script>
  import Field from '@/components/Field'

  export default {
    name: 'App',
    components: {
      Field
    }
  }
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: Arial, "Helvetica Neue", Helvetica, sans-serif;
    line-height: 130%;
  }
</style>
