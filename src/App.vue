<template>
  <StepperContainer 
   :activeIndex="activeIndex"
   :labels="stepsLabels"
   @update:activeIndex="stepUpdated"
  />
</template>

<script>
import StepperContainer from './components/StepperContainer.vue'
import { ref } from '@vue/runtime-core'

export default {
  name: 'App',
  components: {
    StepperContainer
  },
  setup() {

    const activeIndex = ref(0);
    const stepsLabels = ref(['Info', 'Options', 'Finish'])
    const stepUpdated = (value) => {
      if(value < activeIndex.value){
        activeIndex.value = value
      }
    }



    return {
      activeIndex,
      stepUpdated,
      stepsLabels,

    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
