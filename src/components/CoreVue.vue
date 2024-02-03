<script lang="ts">
// @ts-nocheck



import {  ref, } from 'vue';
import * as wordData from '../words.json';






export default{

    data(){
        return{
            words:[],
            time:0,
            timeLoop:null,
            userWord:undefined,
            wordCount: parseInt(localStorage.getItem('wordCount')) || 25,
            records:JSON.parse(localStorage.getItem('records')) || [],
            underlinedIndex:0,
             wordsIndex:0,
            WPM:null,
            acc:null,
            startedTyping: false,
            displayedWords:ref(''),
            classList: ref([]),
            correctScore: 0,
            wrongScore: 0,
            firstCol: localStorage.getItem('Typed') || null,
            secondCol: localStorage.getItem('Base') || null,
            thirdCol: localStorage.getItem('Wrong') || null,
        }
    },
    
    computed: {
    
    
      splitDisplayWords: {
      get(){
        
        if (this.displayedWords != null){

          return this.displayedWords.split('').map((letter, index) => ({
            letter,
            isUnderlined: index === this.underlinedIndex,
            reactive: ref(this.classList), 
          }));
        }else{
          this.randomWords
        }
      },
      set(){
       
        
      }
      }, 
    
    
    


  },
    mounted(){
        //Force focused Input
        this.$refs.inputRef.focus();  
        console.log(localStorage.getItem('wordCount'));
        

        
        this.words = wordData.default   
        //Calls function on load
        this.randomWords()
        setInterval(this.wordCountVal,1500)
        
        
    },
    watch: {
      userWord: 'updateUnderlinedIndex',
      
     

      
      displayedWords(){
        if (this.displayedWords == null){
          this.randomWords
        }
      },
      // Forces Updated Values into localStorage
        firstCol(){
          localStorage.setItem('Typed',this.firstCol)
        },
        secondCol(){
          localStorage.setItem('Base',this.secondCol)
        },
        thirdCol(){
          localStorage.setItem('Wrong',this.thirdCol)
        }
      
      
  },    
    
    methods:{ 
     wordCountVal(){
      const val = localStorage.getItem('wordCount')
      // console.log(val);
      if (val == null){
        return
      }
      else if (val != this.wordCount ){
        this.wordCount = val
        this.handleRestart()
      }
      
      
     },
      randomWords() {
  // Check if this.words is an array
  if (!Array.isArray(this.words)) {
    return [];
  }

    
    
  const shuffledWords = [...this.words];
  // Shuffle the array
  for (let i = shuffledWords.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [shuffledWords[i], shuffledWords[j]] = [shuffledWords[j], shuffledWords[i]];
  }

  //Shift words into random order then slice off 25
  const combinedWords = shuffledWords.map(word => word.word).slice(0,this.wordCount || 25).join(' ')
  this.displayedWords = combinedWords
  
},
      
      updateUnderlinedIndex(){
        
        this.underlinedIndex = this.userWord?.length;
        // LOGS TO CHECK ALL VALUES// 
        // console.log(`input: ${this.userWord} 'correct:' ${this.correctScore} 'wrong' : ${this.wrongScore} `);
        // console.log(`lengths: ${this.userWord.length -1}, ${this.splitDisplayWords.length}`);
        // console.log(this.startedTyping);
        //   console.log(this.correctScore,this.wrongScore);
        // console.log(this.splitDisplayWords[this.wordsIndex]);
              
        
      },  
   
      handleBlur() {
      // Refocus the input when it loses focus
      this.$refs.inputRef.focus();
    },
   
    WpmMath(){
      // Equation based off https://www.speedtypingonline.com/typing-equations 
      this.WPM = (this.correctScore / 5 / (this.time /60))
      this.acc = ((this.userWord.length -this.wrongScore) / this.userWord.length) * 100
      
      
      //  console.log(this.WPM);
      // console.log(this.acc);
      
    },
    
    
    handleInput(e) {
      const currentLetter = this.splitDisplayWords[this.wordsIndex];

        
      
      
  if (!this.startedTyping) {
    this.startTime();
  }

  if (this.userWord.length === this.splitDisplayWords.length) {
    this.submitCompletion();
    const a = this.splitDisplayWords.slice(-1)
    // console.log(a[0].letter);
    // console.log(this.userWord.slice(-1));
      if (this.userWord.slice(-1) === a[0].letter){

        this.classList.push(this.firstCol)    
      }else{

        this.classList.push(this.thirdCol)
      }
    return;
  }
  if (e.data === null){
    this.wordsIndex -= 1
   
    this.classList.pop()
  }

  else if (e.data === currentLetter.letter) {
    // ADD SCORING EACH CORRECT LETTER = character +1 
    this.correctScore += 1;
    this.classList.push(this.firstCol)
    this.wordsIndex += 1;

    
  } else {
    this.wrongScore += 1;
    this.wordsIndex += 1;
    this.classList.push(this.thirdCol)
  }

      },
     
      startTime(){
      if (!this.startedTyping) {
      this.startedTyping = true;
      this.timeLoop = setInterval(() => {

        this.time += .1
      }, 100);
    }
    },
    stopTime(){
      clearInterval(this.timeLoop)
    },
    submitCompletion(){
          console.log('test finished')
          this.stopTime()
          this.WpmMath()
        },
      handleRestart(){
        console.log('restart');
        clearInterval(this.timeLoop)
        this.time = 0
        this.displayedWords = null
        this.randomWords()
        this.wordsIndex = 0
        this.underlinedIndex = 0
        this.userWord = undefined
        this.startedTyping = false
        this.Wpm = null
        this.correctScore = 0
        this.wrongScore = 0
        this.classList.length = 0
        this.acc = null
        
      },
      saveScore(){
        let today = new Date();
let dd = today.getDate();
let mm = today.getMonth() + 1;
let yyyy = today.getFullYear();
if (dd < 10) {
    dd = '0' + dd;
}
if (mm < 10) {
    mm = '0' + mm;
}
today = mm + '-' + dd + '-' + yyyy;
        const Obj = {
          Date: today,
          WPM: this.WPM,
          Accuracy: this.acc,
          Version:this.wordCount,

        }
      console.log(today);
        this.records.push(Obj)
        console.log(this.records);
        localStorage.setItem('records',JSON.stringify(this.records))

        
      },
      handleDelete(index){
        const newRecord = [...this.records];
const filteredRecord = newRecord.filter((record, recordIndex) => recordIndex !== index);

// Update the original array with the filtered one
this.records = filteredRecord;

// Update local storage
localStorage.setItem('records', JSON.stringify(filteredRecord));

      }
    
          
    },
    
  }
  

  
 

    


</script>

<template>
    <div>

      <div class="color">

  <input type="color" id="favcolor" name="favcolor" v-model="firstCol">
  <input type="color"  name="favcolor" v-model="secondCol">
  <input type="color"  name="favcolor" v-model="thirdCol">
      </div>
      <div class="color">
          <div>Typed </div>
          <div>Base </div>
          <div>Wrong </div>
      </div>
        <div class="Core">
        
          <div>{{ time.toFixed(2) }}</div>
          <p class="cont" :style="{color: this.secondCol}">
            <span   v-for="(letter, index) in splitDisplayWords" :key="index">
             
              <span class="letter" :style="{ 'text-decoration': letter.isUnderlined ? 'underline' : 'none', color: classList[index] }">                
                {{ letter.letter }}

              </span>
            </span>
          </p>


            
            <input class='invisInput'
      v-model="userWord"
      ref="inputRef" 
      @input="handleInput"
      @blur="handleBlur"
    >

        </div>
        <div class="Score" >
          <div class="ScoreStat" v-if="this.WPM && this.acc">

            <div>Wpm {{ WPM.toFixed(2) }}</div>
            <div>Acc {{ acc.toFixed(2) }}</div>
          </div>


            <button @click="handleRestart" class="svg"><svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" fill="currentColor" class="bi bi-arrow-clockwise" viewBox="0 0 16 16">
              <path fill-rule="evenodd" d="M8 3a5 5 0 1 0 4.546 2.914.5.5 0 0 1 .908-.417A6 6 0 1 1 8 2z"/>
              <path d="M8 4.466V.534a.25.25 0 0 1 .41-.192l2.36 1.966c.12.1.12.284 0 .384L8.41 4.658A.25.25 0 0 1 8 4.466"/>
            </svg>restart</button>
            <button @click="saveScore" v-if="this.WPM && this.acc" class="save">
              Save Score
            </button>

        </div>  

      </div>
      <div class="recordContainer">
        <div class="record">Records🏆</div>
        <div class="recordList"  v-for="(record,index) in records" :key="index">
          <p>Wpm:{{ record.WPM.toFixed(2) }}</p>
          <p>Accuracy: {{ record.Accuracy.toFixed(2) }}</p>
          <p>{{ record.Date }}</p>
          <p v-if="record.Version == 25">Long</p>
          <p v-else-if='record.Version == 10'>Short</p>
          <button @click="handleDelete(index)" class="recordDelete"><svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" fill="currentColor" class="bi bi-trash" viewBox="0 0 16 16">
  <path d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5m2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5m3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0z"/>
  <path d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1zM4.118 4 4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4zM2.5 3h11V2h-11z"/>
</svg></button>
        </div>

      </div>

</template>

<style scoped>
.color{
  /* background-color: white; */
  /* color:black; */
  display:flex;
  justify-content: center;
  gap:20px;
  text-align: center;

}
.letter{
  font-size:2.2rem;

}

p{
    font-size:2rem;
    font-family: 'Lalezar', system-ui;
    color:firstCol
}
.blue{
  color:blue;
}
.Score{
  display: flex;
  justify-content: center;
  gap: 10px;
  }
  .save{
      height: 55px;
      align-self: center;

    
  }
  .button{
    font-family: 'Lalezar', system-ui;
  }
  .ScoreStat{
margin-right:20px;
  /* gap:25px; */
font-size:2rem;
font-family: 'Lalezar', system-ui;

  }
  .svg{
    height:55px;
    width:55px;
    align-self: center;
    cursor: pointer;
    background-color: rgb(129, 131, 134);
    border:none;

    border-radius: 5px;
  }
  .Core{
    display:flex;
    justify-content: center;
    margin:20px 0px 0px;
    user-select:all;
    text-align: center;
}
/* p:focus{
    background-color: white;;
} */
.invisInput{
  opacity:0;
  z-index:-10;
}

.accurate{
  color:red;
  /* background-color: red; */
}
.underline{
  /* text-decoration: underline 5px black; */
  transition: width 0.3s cubic-bezier(0.68, -0.55, 0.27, 1.55);
  border-bottom:5px solid black;
  border-radius: 5px;   

}
.recordContainer{
  margin:0 15%;


}
.record{
  display:flex;
  justify-content: center;
  font-family: Lalezar;
  font-size:2rem;
}
.recordDelete{
  height:fit-content;
  align-self: center;
  background-color: inherit;
  border:none;
  cursor: pointer;
}
.recordList{
  display:flex;
  justify-content: space-between;
  margin:5px 0;
  padding:0 20px;
  background-color: rgb(39, 40, 41);
}
</style>