<template>
  <div class="calculadora">
    <div class="expressao">
      <small>{{ remainder }}</small>
      <div v-if="answer">{{ answer }}</div>
      <div v-else>{{ expressao }}</div>
    </div>
    <div class="operadores">
      <button v-on:click="adicionarOperador(numero)" v-for="numero, index in operacoes" :key="index">
        {{ numero }}
      </button>
      <button @click="handleEqualto">=</button>
      <button v-on:click="handleClear">
          Clear
      </button>
    </div>
  </div>
</template>

<script>
import { OPERACOES, OPERADORES_MATEMATICOS } from './constants/operacoes'

export default {
  name: 'App',
  data:function(){
    return{
      operacoes: OPERACOES,
      operators: '',
      expressao: '',
      remainder: '',
      answer:0
    }
  },
  methods:{
    handleClear(){
      this.operators = '',
      this.expressao = '',
      this.remainder = '',
      this.answer = 0
    },
    handleEqualto(){
      console.log('asdf ===')
      if(this.remainder && this.expressao && this.operators){
        this.answer = this.handleCalculate(this.remainder, this.expressao, this.operators)
        this.remainder = this.remainder+' '+this.expressao
        this.expressao = '';
      }
    },
    adicionarOperador(numero){
      if(OPERADORES_MATEMATICOS.indexOf(numero) !== -1){
        if(((this.operators == numero) && this.operators) && this.expressao){
          if(this.answer){
            this.answer = this.handleCalculate(this.answer, this.expressao, this.operators)
            this.remainder = this.answer+' '+this.operators+' '+this.expressao
          }else{
            console.log('asdf2');
            this.answer = this.handleCalculate(this.remainder, this.expressao, this.operators)
            this.remainder = this.remainder+' '+this.expressao
            this.expressao = '';
          }
          return
        }else if(((this.operators == numero) && this.operators) && this.expressao == ''){
          console.log('asdf_new');
          this.remainder = this.answer+' '+this.operators;
          this.answer = 0;
        }else if((this.operators != numero && this.operators) && this.remainder){
          if(OPERADORES_MATEMATICOS.indexOf(numero) !== -1){
            this.operators = numero;
          }
          this.remainder = this.answer+' '+this.operators;
          this.answer = 0
        }else{
          console.log('asdf3');
          this.operators = numero
          this.remainder = this.expressao+' '+this.operators
        }
        this.expressao = '';
      }else{
        console.log('asdf4');
        if(this.remainder){
          console.log('asdf5');
          if(this.answer){
            console.log('asdf6');
            // this.remainder = this.answer
            this.remainder = '';
            this.expressao += numero
            this.operators = '';
            this.answer = 0;
            return
          }else{
            console.log('asdf7');
            // this.expressao = '';
          }
        }
        this.expressao += numero
      }
    },
    handleCalculate(sum1, sum2, operator){
        const val1 = parseFloat(sum1)
        const val2 = parseFloat(sum2)
        let result = 0
        switch(operator){
          case '+':
            result = val1+val2;
            break;
          case '-':
            result = val1-val2;
            break;
          case '*':
            result = val1*val2;
            break;
          case '/':
            result = val1/val2;
            break;
        }
        return result;
    }
  },
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
.calculadora {
    height: 70vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}

.expressao {
    max-width: 392px;
    width: 100%;
    min-height: 52px;
    margin-bottom: 16px;
    font-size: 1.25rem;
    text-align: right;
    padding: 6px;
    border-radius: 8px;
    background-color: #1a1a1a;
    display: flex;
    flex-wrap: wrap;
    justify-content: flex-end;
    align-items: flex-start;
    flex-direction: row;
}
.expressao small{width:100%}

.operadores {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 8px;
    max-width: 400px;
    width: 100%;

}

.botao,
.botao-maior {
    display: flex;
    justify-content: center;
}

.botao-maior {
    grid-column-start: 1;
    grid-column-end: 3;
}

.submit {
    width: 100%;
    max-width: 400px;
    margin-top: 16px;
    display: flex;
    justify-content: center;
}
</style>
