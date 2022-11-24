<template>
  <div class="game">
    <h3>Guess the color!</h3>
    <div class="color" :style="colorBoxStyle">   </div>
    <div class="buttons">
      <button @click="optionClicked(0)">{{ buttonOptions[0] }}</button>    
      <button @click="optionClicked(1)">{{ buttonOptions[1] }}</button>   
      <button @click="optionClicked(2)">{{ buttonOptions[2] }}</button>
    </div>
    <div class="result" :style="resultStyle">{{ result }}</div>  
  </div>
</template>

<script>
import { reactive, ref } from '@vue/runtime-core'

export default {
  setup() {
    const color = ref('#ffffff')
    const buttonOptions = ref(['#ffffff', '#ffffff', '#ffffff'])
    const colorBoxStyle = reactive({ background: color.value, color: color.value })
    const result = ref('Click any button to start!')
    const resultStyle = reactive({ color: '#4f3c66' })

    const generateHexColor = () => {
      return '#' + Math.floor(Math.random()*16777215).toString(16);
    }

    const updateColor = () => {
      colorBoxStyle.background = color.value
      colorBoxStyle.color = color.value
    }

    const updateResultColor = () => {
      if (result.value === 'Correct!') {
        resultStyle.color = '#22ff22'
      } else if (result.value === 'Try again') {
        resultStyle.color = '#e24b4b'
      } else if (result.value === 'Start!') {      
        resultStyle.color = '#f8f8f8'
      }
    }

    const optionClicked = (id) => {
      if (buttonOptions.value[id] === color.value){
        if (color.value === '#ffffff') {
          result.value = 'Start!'
        } else {
          result.value = 'Correct!'
        }
        updateResultColor()
        
        for (let i = 0; i < 3; i++) {
          buttonOptions.value[i] = generateHexColor()
        }
        
        //NotLikeThis
        let rand = Math.random()
        if (rand <= 0.33) {
          color.value = buttonOptions.value[0]
        } else if ((rand >= 0.33) & (rand <= 0.67)) {
          color.value = buttonOptions.value[1]
        } else {
          color.value = buttonOptions.value[2]
        }       
        updateColor()
      } else {
        result.value = 'Try again'
        updateResultColor()
      }
    }

    return { colorBoxStyle, optionClicked, buttonOptions, result, resultStyle }
  }
}
</script>

<style>
.game {
  background: #f8f8f8;
  margin: 25px auto;
  border-radius: 30px;
  max-width: 40%;
  min-width: 300px;
  padding: 20px 20px;
}

.game h3 {
  margin: 20px auto 0px;
  padding: 10px;
}

.game .color {
  max-width: 30%;
  padding: 30px 30px;
  border-radius: 10px;
  margin: 0px auto 20px;
  transition: 0.5s;
}

.buttons {
  display: block;
  margin: 10px auto;
  background: #ebebeb;
  border-radius: 30px;
  padding: 10px 10px;
  max-width: 80%;
}

.buttons button {
  color: #4f3c66;
  background: #d6d6d6;
  margin: auto 20px;
  border: none;
  border-radius: 20px;
  padding: 10px 10px;
  cursor: pointer;
  font-weight: bold;
  font-size: 1em;
  transition: 0.2s;
}

.buttons button:hover {
  background: #f5f5f5;
}

.result {
  padding: 10px 10px;
  font-weight: bold;
  font-size: 2em;
  transition: 0.3s;
}
</style>