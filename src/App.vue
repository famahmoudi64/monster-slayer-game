<template>
  <header>
    <h1 class="mons">Monster Slayer</h1>
  </header>
  <section class="container">
    <h1>Monster Health</h1>
    <div class="healthbar">
      <div class="healthbar-value" :style="monsterbarstyles"></div>
    </div>
  </section>
  <section class="container">
    <h1>Player Health</h1>
    <div class="healthbar">
      <div class="healthbar-value" :style="playerbarstyles" ></div>
    </div>
  </section>
  <section class="container1" v-if="winner">
    <h2>Game Over</h2>
    <h3 v-if="winner==='player'">"you win"</h3>
    <h3 v-else-if="winner==='monster'">"you lost"</h3>
    <h3 v-else>"its a draw"</h3>
    <button @click="newstart">New Start</button>
  </section>
  <section id="controls" v-else>
    <button @click="attackmonster">Attack</button>
    <button @click="specialattackmonster" :disabled="myspecialattack" :style="myspecialattackcolor">Special Attack</button>
    <button @click="healplayer">Heal</button>
    <button @click="surrender">Surrender</button>
  </section>
  <section class="container">
    <h2>Battle Log</h2>
    <ul class="list">
      <li v-for="logmessage in logmessages">
       <span :class="{'logo--me':logmessage.actionby==='player' , 'logo--you':logmessage.actionby==='monster'}">{{logmessage.actionby}}</span>
       <span v-if="logmessage.actiontype==='heal'"> himself for <span class="logogreen">{{logmessage.actionvalue}}</span></span>
       <span v-else="logmessage.actiontype==='attack'"> attacks <span class="logoorange">{{logmessage.actionvalue}}</span></span>
       </li>
    </ul>
  </section>
</template>

<script>
export default {
  name: 'App',
data(){
  return{
    playerhealth:100,
    monsterhealth:100,
    currentround:0,
    winner:null,
    logmessages:[]
  }
},
watch:{
  playerhealth(value){
  if(value<0 && this.monsterhealth<0){
    this.winner='draw'
  }else if(value<0){
   this.winner='monster'
  }
  },
  monsterhealth(value){
    if(value<0 && this.playerhealth<0){
      this.winner='draw'
    }else if(value<0){
      this.winner='player'
    }
  }
},
computed:{
  monsterbarstyles(){
    if(this.monsterhealth<0){
      return {width:'0%'}
    }
    return {width:this.monsterhealth + '%'}
  },
  playerbarstyles(){
    if(this.playerhealth<0){
      return{width:'0%'}
    }
   return {width:this.playerhealth + '%'}
  },
  myspecialattack(){
    return  this.currentround % 3 !==0 
  },
  myspecialattackcolor(){
   return {backgroundColor:this.currentround % 3 !==0?'gray' : 'tomato'}
  }  
},
methods:{
  attackmonster(){
    this.currentround++
     const attackvalue=Math.floor(Math.random() * (12-5))+5;
    this.monsterhealth-=attackvalue;
    this.addlog('player','attack',attackvalue)
   this.attackplayer()
  },
  specialattackmonster(){
    this.currentround++
  const attackvalue=Math.floor(Math.random()*(20-15)+15)
  this.monsterhealth-=attackvalue
  this.addlog('player','attack',attackvalue)
  this.attackplayer()
  },
  healplayer(){
    this.currentround++
    const healvalue=Math.floor(Math.random()*(10-6)+6)
    if(this.playerhealth+healvalue >100){
      this.playerhealth=100;
    }else{
      this.playerhealth += healvalue;
    }
    this.addlog('player','heal',healvalue)
  },
  attackplayer(){
    const attackvalue=Math.floor(Math.random() * (15-8))+8;
    this.playerhealth-=attackvalue;
    this.addlog('monster','attack',attackvalue)
  },
  newstart(){
   this.playerhealth=100
    this.monsterhealth=100
    this.winner=null
    this.currentround=0
    this.logmessages=[]
  },
  surrender(){
    this.winner='monster'
  },
  addlog(who,what,value){
   this.logmessages.unshift({
    actionby:who,
    actiontype:what,
    actionvalue:value
   })
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
#controls{
  display:flex;
  flex-direction: column;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
  margin-top:50px;
}
.list{
  font-size:20px;
}
.logo--me{
  color:red
}
.logo--you{
  color:purple
}
.mons{
font-size:30px;
}
h2{
 padding-top:10px;
}
.logogreen{
  color:green
}
.logoorange{
  color:orange
}
button{
  background-color: tomato;
  width:200px;
  height:50px;
  font-size: 20px;
  color:white;
  border:1px solid tomato;
  border-radius: 10px;
  margin-bottom: 15px;
  box-shadow:2px 2px 2px rgb(234, 146, 246)
}
header{
  background-color: darkviolet;
  color:white;
  height:70px;
  border-radius: 10px;
  margin:auto;
  text-align: center;
  justify-content: center;
}
.container{

background-color: antiquewhite;
margin:10px auto;
border-radius: 10px;
box-shadow: 2px 3px gainsboro
}
.container1{
  /* width:80rw; */
  background-color:cadetblue;
  margin: 10px auto;
  border-radius: 10px;
  font-size:20px;
  box-shadow: 2px 3px rgb(77, 122, 123);
}
.healthbar{
  width:90%;
  height:50px;
  text-align: center;
  justify-content: center;
  align-content: center;
  margin:auto;
}
.healthbar-value{
  width:100%;
  height:100%;
  background-color:forestgreen;
  border-radius: 10px;
}
li{
  list-style: none;
}
</style>
