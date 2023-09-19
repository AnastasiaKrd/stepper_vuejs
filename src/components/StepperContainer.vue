<template>
  <section class="stepper">
    <ul class="stepper-container">
      <li class="item-and-line" v-for="(label, i) in labels" :key="label">
        <div class="item"
          :class="{'completed' : (index > i), 'active' : (index === i)}"
        >
          <div class="count">{{i + 1}}</div>
          <div class="label">{{label}}</div>
        </div>
        <div class="item" v-if="i < labels.length - 1">
          <div class="line"></div>
        </div>
      </li>
    </ul>
  </section>
  <div class="step-content">
    <div v-if="index === 0">
      <form>
        <label for="fname">Name</label><br>
        <input type="text" id="fname" name="fname">
        <br>
        <label for="femail">e-mail</label><br>
        <input type="text" id="femail" name="femail">
      </form>
    </div>
    <div v-if="index === 1">
      <form>
        <input type="radio" id="op1" name="options" value="Option 1">
        <label for="op1">Option 1</label><br>
        <input type="radio" id="op2" name="options" value="Option 2">
        <label for="op2">Option 2</label><br>
        <input type="radio" id="op3" name="options" value="Option 3">
        <label for="op3">Option 3</label>
      </form>
    </div>
    <div v-if="index === 2">
      <h3>Finish!</h3>
    </div>
  </div>

    <div class="footer">
      <div class="back">
        <button type="button" v-if="index > 0" class="steps-button" @click="handlePrevious">Previous</button>
      </div>
      
      <div class="forward">
        <button v-if="index < labels?.length - 1" type="button" class="steps-button" @click="handleNext()">Next</button> 
         
      </div>
    </div>  
  

</template>

<script>
import { ref, onMounted } from '@vue/runtime-core'


export default {
  components: {
    
  },
  props: {
    activeIndex: {
      type: Number,
      default: 0,
      required: true,
      validator(value){
        return value >= 0
      }
    },
    labels: {
      type: Array,
      default: () => [],
      required: true
    },
  },
  emits: ['update:activeIndex'],

  setup(props, {emit}) {

    const index= ref(0);

    onMounted(()=>{
      if(props.activeIndex > props.labels.length || props.activeIndex < 0){
        console.error("ActiveIndex out of bounds")
      }
      
    })
   
    
    const handlePrevious = () => {
      if (index.value !=0) {
        index.value=index.value -1
        emit('update:activeIndex', index.value)
      }
    }

    const handleNext = () => {
      if (index.value != props.labels.length) {
        index.value = index.value +1
        emit('update:activeIndex', index.value)    
      }
    }
    
    return {
      index,
      handlePrevious,
      handleNext
    }
  }
}
</script>

<style scoped >


.stepper{
  width: 70%;
  display: flex;
  justify-content: center;
  margin: auto;
}
.stepper-container{
  display: flex;
  justify-content: center;
  align-items: flex-start;
  padding: 10px 10px;
  list-style-type: none;
  position: relative;
}
.item{
  flex-basis: 0;
  -webkit-box-flex: 1;
  -ms-flex-positive: 1;
  flex-grow: 1;
  max-width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
  min-height: 32px;
  position: relative;
  padding: 0 10px
}
.item-and-line{
  display: flex;
  justify-content: center;
  align-items: flex-start;
  position: relative;
}
.line{
  display: flex;
  align-items: center;

  overflow: hidden;
  width: 100%;
  min-width: 100px;
  margin-left: -20px;
  margin-right: -20px;
  background-color: silver;
  height: 2px;
}
.count{
  height: 32px;
  width: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  margin: 0 auto;
  position: relative;
  color: white;
  background: #6F6F6F;
}
.item.completed {
  cursor: pointer;
}
.completed .count {
  background: #6d5dd6;
}
.active .count{
  background: rgb(66, 171, 66);
}
.label{
  min-height: 32px;
  width: 32px * 2;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  margin: 0 auto;
  position: relative;
  color: black;
}
.footer{
  border-top: 2px solid silver;
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 70%;
  margin: auto;
  margin-top: 20px;
  padding: 32px 48px;
  height: 10%;
}
  .steps-button{
    width: 6rem;
    height: 3rem;
    background-color: #1d9697;
    color: beige;
    border: none;
    border-radius: 10%;
  }

  .back{
    justify-self: flex-start;
  }
  .forward{
    display: flex;
    justify-content: flex-end;
    align-items: center;
    column-gap: 1rem;
  }

  .step-content{
    border-color: black;
    border-radius: 20%;
    height: 300px;
    width: 300px;
    background: linear-gradient(0deg, rgba(34,193,195,1) 0%, rgba(253,187,45,1) 100%);
    margin: auto;
    padding: 20px;
    justify-content: center;
  }

  form{
    margin: 20%;
  }


</style>
