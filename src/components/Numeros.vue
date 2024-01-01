<template>
  <div>
    <h3>BingoScript:</h3>
    <div id="maindiv">
      <div id="left">
        <div id="tablenumbers">
          <table id="tablenumberstable">
            <tr v-for="m in 8" :key="m">
              <td class="cell" v-for="n in 10" :key="n" >
                <div :class="checkNumber(m,n)?' called':''" >
                  <div :class="lastCalledNumber==(10*(m-1)+n)?' actualNumber':''">
                    {{10*(m-1)+n}}
                  </div>
                </div>
              </td>
            </tr>
          </table>
        </div>
      </div>
      <div id="right">
        <button v-if="!automaticCall" v-on:click="callNumber">siguiente Numero</button>
        <button v-if="!automaticCall" v-on:click="callNumbersAutomatic">sacar numeros automaticamente</button>
        <button v-if="automaticCall" v-on:click="stopNumbersAutomatic">parar numeros automaticamente</button>
        <div id="last">
          <h4>Ultimo Numero:   </h4>
          <div>
            <h1 id="lastNumber">{{lastCalledNumber}}</h1>
          </div>
        </div>
        <div>
          <h4>Numeros Anteriores:</h4>
          <div>
            <ul>
              <li class="none" v-for="element in lastCalledNumbers" :key="element.id">{{ element }}</li>
            </ul>
          </div>
        </div>
      </div>
    </div>
    <br>
    <div>
      faltan {{remainingNumbers}} numeros por salir
    </div>
  </div>
</template>
<script>
const synth = window.speechSynthesis 

  export default {
    name: 'Numeros',
    data() {
      return {
        notCalledArray :null,
        calledArray :[], 
        lastCalledNumber: null, 
        lastCalledNumbers: [],
        remainingNumbers: 80,
        interval: null,
        automaticCall: false
      }
    }, 
    methods: {
      callNumber: function() {
        if(this.notCalledArray.length>0){
          var randomIndex= Math.floor(Math.random() * this.notCalledArray.length)
          console.log("selected number:")
          console.log(this.notCalledArray[randomIndex])
          this.lastCalledNumber=this.notCalledArray[randomIndex]
          if(this.lastCalledNumbers.length<=4){
            this.lastCalledNumbers.push(this.notCalledArray[randomIndex])
          }else{
            this.lastCalledNumbers.splice(0,1)
            this.lastCalledNumbers.push(this.notCalledArray[randomIndex])
          }
          this.calledArray.push(this.notCalledArray[randomIndex])
          this.notCalledArray.splice(randomIndex,1)
          this.remainingNumbers= this.notCalledArray.length


          // --------------

          var text = this.lastCalledNumber
          if (this.lastCalledNumber >=60 && this.lastCalledNumber <70 ){
            text=text + ", 6, " + (this.lastCalledNumber-60) 
          }else if( this.lastCalledNumber >=70 && this.lastCalledNumber <80 ){
            text=text + ", 7, " + (this.lastCalledNumber-70) 
          }
          
          const utterThis = new SpeechSynthesisUtterance(text) 
          synth.speak(utterThis)
        }else{
          text="Bingo finalizado, ya no quedan más números en el bombo"
          const utterThis = new SpeechSynthesisUtterance(text) 
          synth.speak(utterThis)
          this.automaticCall=false
          clearInterval(this.interval)
          alert("ya no quedan más números en el bombo")
        } 
      },
      callNumbersAutomatic: function(){
        this.automaticCall=true
        var text=null
        if (this.calledArray.length==0){
          text = "¡Empezamos el bingo!"
        }else{
          text = "continuamos"
        }
        
        const utterThis = new SpeechSynthesisUtterance(text) 
        synth.speak(utterThis)
        this.interval=setInterval(this.callNumber,4000)
      },
      stopNumbersAutomatic: function(){
        this.automaticCall=false
        clearInterval(this.interval)
        var text="bingo pausado"
        const utterThis = new SpeechSynthesisUtterance(text) 
        synth.speak(utterThis)
      },
      checkNumber: function(number1,number2){
        var resultado= 10*(number1-1)+number2
        return this.calledArray.includes(resultado)
      }
    },
    mounted() {
      this.notCalledArray=Array(80).fill().map((v,i)=>i+1)
      this.calledArray=[]
      console.log("arrays inicializados:")
      console.log(this.notCalledArray)
    }
  }
</script>
<style>
  h3 {
    margin-bottom: 5%;
    color: blue;
  }
  .cell {
    color: black;
    padding: 10px;
    font-size: 30px;
    font-weight: bold;
  }
  .called {
    color: rgb(255, 255, 255);
    background: rgb(255, 108, 10);
  }
  #maindiv {
    display: flex;
  }
  #right{
    width: 30%;
    text-align: center;
  }
  #left {
    width: 70%;
    align-content: center;
  }
  #tablenumbers {
    width: 90%;
  }
  #tablenumberstable {
    width: -webkit-fill-available;
  }
  #lastNumber{
    font-size: 700%;
  }
  .none{
  list-style-type: none;
  }
  .actualNumber {
    background: rgb(0, 211, 70);
  }
</style>