<template>

  <section class="src-components-juego">
    <div id="header">
      <h1>The Great <br>
          <span id="colorDisplay">{{pickedColor}}</span>
          <br>
          Guessing Game
      </h1>
    </div>

    <div id="navigator">
      <button id="reset" @click="restart">{{restartButton}}</button>
      <span id="message">{{mensaje}}</span>

      <button id="easy" @click="easyButton">Easy</button>
      <button id="hard" @click="hardButton" class="selected">Hard</button>
    </div>

    <div id="container">
      <div class="square" :style="{'background-color':this.colors[0]}" @click="correcto(0)"></div>
      <div class="square" :style="{'background-color':this.colors[1]}" @click="correcto(1)"></div>
      <div class="square" :style="{'background-color':this.colors[2]}" @click="correcto(2)"></div>
      <div class="square" :style="{'background-color':this.colors[3]}" @click="correcto(3)"></div>
      <div class="square" :style="{'background-color':this.colors[4]}" @click="correcto(4)"></div>
      <div class="square" :style="{'background-color':this.colors[5]}" @click="correcto(5)"></div>
    </div>
  </section>

</template>

<script lang="js">

  export default  {
    name: 'src-components-juego',
    props: [],
    mounted () {
      this.colors = this.createNewColors()
      this.pickedColor = this.colors[this.pickColor()]
    },
    data () {
      return {
        colorCount: 6,
        isHard: true,
        colors: [],
        pickedColor: 0,
        mensaje: '',
        restartButton: 'New Colors'
      }
    },
    methods: {
      start(){
        this.colors = this.createNewColors()
        console.log(this.colors)
      },
      createNewColors(){
        let resul = []
        for(let i = 0; i < this.colorCount; i++){
          resul.push(this.createRandomStringColor())
        }
        return resul
      },
      createRandomStringColor(){
        return 'rgb('+ this.randomInt() + ", " + this.randomInt() + ", " + this.randomInt() + ")"
      },
      randomInt(){
        return Math.floor(Math.random()*256)
      },
      pickColor(){
        let quantity
        this.isHard ? quantity = 6 : quantity = 3
        return Math.floor(Math.random() * quantity)
      },
      setAllColorsTo(){
        let squares = document.querySelectorAll('.square')
        for(let i = 0; i < this.colorCount; i++){
          squares[i].style.backgroundColor = this.pickedColor
        }
      },
      restart(){
        this.colors = this.createNewColors()
        this.pickedColor = this.colors[this.pickColor()]
        this.mensaje = ''
        this.restartButton = 'New Colors'
      },
      easyButton(){
        if(this.isHard){
          this.isHard = false
          this.colorCount = 3
          this.restart()
          document.querySelector('#hard').classList.remove('selected')
          document.querySelector('#easy').classList.add('selected')
        } else {
          this.restart()
        }
      },
      hardButton(){
        if(!this.isHard){
          this.isHard = true
          this.colorCount = 6
          this.restart()
          document.querySelector('#easy').classList.remove('selected')
          document.querySelector('#hard').classList.add('selected')
        } else {
          this.restart()
        }
      },
      correcto(pos){
        console.log(this.colors[pos])
        if(this.colors[pos] == this.pickedColor){
          this.setAllColorsTo(this.colors[pos])
          this.mensaje = 'You pick the right!'
          this.restartButton = 'Play Again!'
        } else {
          document.querySelectorAll('.square')[pos].style.backgroundColor = '#fff'
          this.mensaje = 'Try Again!'
        }
      },
    },
    computed: {

    }
}
</script>

<style lang="css">
  .src-components-juego {
    color: #fff;
  }
  h1 {
    font-weight: normal;
    line-height: 1.1;
    padding: 20px 0;
  }
  #navigator {
    background: #fff;
    height: 30px;
    text-align: center;
    margin: 0;
    margin-top: -30px;
  }
  #header {
    transition: all 0.3s;
    background: steelblue;
    text-transform: uppercase;
    text-align: center;
    margin: 0;
    color: #fff;
  }
  #colorDisplay {
    font-size: 200%;
  }
  .square {
    width: 30%;
    padding-bottom: 30%;
    float: left;
    margin: 1.66%;
    border-radius: 10%;
    transition: background 0.8s;
    -webkit-transition: background 0.8s;
    -moz-transition: background 0.8s;
    cursor: pointer;
  }
  #container {
    margin: 20px auto;
    max-width: 600px;
  }
  #message {
    color: #000;
    display: inline-block;
    width: 20%;
  }
  .selected {
    background-color: steelblue;
    color: white;
  }
  button {
    border: none;
    background-color: white;
    font-family: "Montserrat", "Avenir";
    text-transform: uppercase;
    height: 100%;
    font-weight: 700;
    letter-spacing: 1px;
    color: steelblue;
    transition: all 0.3s;
    outline: none;
  }
  button:hover {
    color: white;
    background-color: steelblue;
  }
</style>
