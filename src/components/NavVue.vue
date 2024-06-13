<script lang="ts">
import axios from 'axios';
import { ref } from 'vue';


export default{
    data(){
        return{
            switch:true,
            wordCount: localStorage.getItem('TestVer'),
            color: ref(localStorage.getItem('Base')),
            logo:localStorage.getItem('Typed'),
            leaderboard:null,
            isActive: false,
            username:localStorage.getItem('Username') || null,

        }
    },

    methods:{
        formatDate(dateString) {
      const date = new Date(dateString);
      const day = date.getDate();
      const month = date.getMonth() + 1; // January is 0
      const year = date.getFullYear();
      return `${day}/${month}/${year}`;
    },
        async HandleLeaderboard(){
             this.isActive = this.isActive ? false : true;
            try{
                const response = await axios.post('http://localhost:3000/data',{username:this.username});
                // const response = await axios.post('https://gorillatypeserver-production.up.railway.app/data',{username:this.username});
                this.leaderboard = response.data
                // console.log(response)
                console.log(this.leaderboard[0].date_submitted)
            }catch(error){
                console.error('Error Getting Data',error)
                
            }
        },     
        updateCol(){
            this.color = localStorage.getItem('Base')
            this.logo = localStorage.getItem('Typed')
            // console.log('yo');
            
        },
        handleWordCount(value){
            // console.log(value);
            localStorage.setItem('wordCount',value)
            
        }
        
    },
    mounted(){
       
        setInterval(this.updateCol,1500)
        
        // console.log(this.username)
    },
    
    
}

</script>


<template>
    <div>
        <div class="NavBar">
            <div class="NavBar-Right">  

                <svg :style='{fill: this.logo}' fill="#000000" height="80px" width="80px" version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 433.632 433.632" xml:space="preserve"><g><path d="M411.999,130.626H73.567v-5.002c0-5.375,4.373-9.749,9.749-9.749h282.935c13.646,0,24.749-11.103,24.749-24.749c0-13.647-11.103-24.75-24.749-24.75h-68.445c-4.143,0-7.5,3.358-7.5,7.5s3.357,7.5,7.5,7.5h68.445c5.376,0,9.749,4.374,9.749,9.75c0,5.375-4.373,9.749-9.749,9.749H83.316c-13.646,0-24.749,11.103-24.749,24.749v5.002H21.634C9.705,130.626,0,140.332,0,152.26v193.362c0,11.929,9.705,21.633,21.634,21.633h390.365c11.929,0,21.633-9.705,21.633-21.633V152.26C433.632,140.332,423.928,130.626,411.999,130.626z M418.632,345.622c0,3.658-2.976,6.633-6.633,6.633H21.634c-3.658,0-6.634-2.976-6.634-6.633V152.26c0-3.658,2.976-6.634,6.634-6.634h390.365c3.657,0,6.633,2.976,6.633,6.634V345.622z"/><path d="M401.132,155.627H32.5c-4.143,0-7.5,3.358-7.5,7.5v171.628c0,4.142,3.357,7.5,7.5,7.5h368.632c4.143,0,7.5-3.358,7.5-7.5V163.127C408.632,158.985,405.274,155.627,401.132,155.627z M285.755,170.627h46.438v27.875h-46.438V170.627z M101.438,256.443h46.438v27.937h-46.438V256.443z M70.72,241.443v-27.941h46.438v27.941H70.72z M132.158,241.443v-27.941h46.438v27.941H132.158z M162.877,256.443h46.439v27.937h-46.439V256.443z M193.597,241.443v-27.941h46.438v27.941H193.597z M224.316,256.443h46.438v27.937h-46.438V256.443z M255.035,241.443v-27.941h46.438v27.941H255.035z M285.755,256.443h46.438v27.937h-46.438V256.443z M316.474,241.443v-27.941h32.605v27.941H316.474z M224.316,170.627h46.438v27.875h-46.438V170.627z M162.877,170.627h46.439v27.875h-46.439V170.627z M101.438,170.627h46.438v27.875h-46.438V170.627z M40,170.627h46.438v27.875H40V170.627z M40,213.502h15.72v27.941H40V213.502z M40,256.443h46.438v27.937H40V256.443z M55.72,327.255H40V299.38h15.72V327.255z M117.158,327.255H70.72V299.38h46.438V327.255z M178.597,327.255h-46.438V299.38h46.438V327.255z M240.035,327.255h-46.438V299.38h46.438V327.255z M301.474,327.255h-46.438V299.38h46.438V327.255z M362.912,327.255h-46.438V299.38h46.438V327.255z M393.632,327.255h-15.72V299.38h15.72V327.255z M393.632,284.38h-46.438v-27.937h9.386c4.143,0,7.5-3.358,7.5-7.5v-35.441h29.553V284.38z M393.632,198.502h-46.438v-27.875h46.438V198.502z"/><path d="M270.119,76.745c-0.022-0.052-0.035-0.082-0.043-0.102C270.114,76.733,270.25,77.053,270.119,76.745z"/><path d="M282.35,79.185c4.21-4.337,1.792-11.93-4.298-12.735c-5.578-0.738-10.16,5.034-7.932,10.295C272.181,81.619,278.56,82.859,282.35,79.185z"/><path d="M270.076,76.643C270.065,76.616,270.061,76.608,270.076,76.643L270.076,76.643z"/></g></svg>

                <p :style="{color: this.color}">gorillatype</p>
                <p @click="HandleLeaderboard" class="leaderboard">👑  </p>
                 
            </div>
            <div class="NavBar-Right">
                
                <p></p>
                <p class="switch" :style="{color:this.logo}"><div  @click="handleWordCount(10)" class="testver">Short</div> | <div  @click="handleWordCount(25)" class="testver">Long</div></p>
            </div>
        </div>
    </div>
    <div class="modal" v-if="isActive">
        <div class="closed">
        <p class="closedTitle">Closed Temporarily</p>
        <button @click="isActive = false" class="exit"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-x-circle-fill" viewBox="0 0 16 16"><path d="M16 8A8 8 0 1 1 0 8a8 8 0 0 1 16 0M5.354 4.646a.5.5 0 1 0-.708.708L7.293 8l-2.647 2.646a.5.5 0 0 0 .708.708L8 8.707l2.646 2.647a.5.5 0 0 0 .708-.708L8.707 8l2.647-2.646a.5.5 0 0 0-.708-.708L8 7.293z"/></svg></button>
        </div>
        <div class="modal-content">
            <div>Short Test Only For Now</div>
            <!-- <p class="closed">Closed Temporarily</p> -->
               
            <div class="refresh">may have to refresh to see ranked position</div>
            </div>
        <div v-for="(item,index) in leaderboard" :key="item.username">
        <div class="scores" :class="{ pr: item.username === username }">
            <div>#{{ item.username === username ? item.row_num : index + 1 }}</div>

            <div  class="scores-detail">{{ item.username }} </div>
            <div class="scores-detail">WPM :{{ item.wpm_score }}</div>
            <div class="scores-detail">ACC :{{ item.accuracy }}</div>
            <div class="scores-detail">{{ formatDate(item.date_submitted) }}</div>

        </div>
        </div>
</div>

</template>

<style>
.closedTitle{
    font-size:24px;
    font-family: 'Lalezar', system-ui;
    color:red;
}
.exit{
    align-self: center;
    /* display: flex; */
    /* justify-self: end; */
}
p{
    font-size:3em;
    font-family: 'Lalezar', system-ui;
} 
.leaderboard{
    cursor:pointer;
}
.closed{
        font-size:24px;
        display:flex;
        justify-content: space-between;
}

.NavBar{
    justify-content:space-evenly;
    display: flex;
}

.NavBar-Right{
    display: flex;
    align-items: center;
}
img{
    /* height:3.5rem; */
    width:5rem;
}
.switch{
    display:flex;
    
}
.testver{
    cursor: pointer;
}
.modal{
    /* display:flex; */
    position: fixed;
    background-color: rgba(0, 0, 0, 0.5); /* Semi-transparent black background */
    height: 70%;
    width: 50%;
    overflow-y: scroll;
   

    top:10%;
    left:50%;
    transform: translateX(-50%);

}
.modal-content{
    justify-content: space-between;
    display:flex;
    font-size:2rem;

    font-family: 'Lalezar', system-ui;
}
.scores{
    display:flex;
    margin:0 10px;
    justify-content: center;
    gap:5px;
    justify-content: space-between;
}
.scores-detail{
    /* gap:5px; */
    max-width:8rem;
}
.pr{
    background-color: white;
    color:black;
}
.refresh{
    font-size:18px;
}
</style>