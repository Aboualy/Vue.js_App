<template>
  <div id="app" >

      <Heading>Heading</Heading>
      <Form>Hello</Form>


  </div>




</template>





<script>
    import Form from "./components/Form";
    import Heading from "./components/Heading";
    import GridList from "./components/GridList";
export default {
  name: 'app',
  components: {
      GridList,

      Heading,
      Form,

  },

}
</script>


<style>
    @import './components/style/App.css';

</style>
