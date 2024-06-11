
<script lang="ts">

import axios from 'axios';



export default{
    props:{
        background: String
    },
    data(){
        return{
            email:'',
            backCol: this.background,           
            
        }
        
    },
    methods:{
        async SendEmail(){
            // e.preventDefault();
            
            try{
                
                const send = await axios.post('https://gorillatypeserver-production.up.railway.app',{email: this.email,subscribed:'subscribed'})
                console.log('success',send.data);

                
            }catch(error){
                console.log(error);
                
            }

            

                

        },

    },
    watch:{
        email(newvalue){
            console.log(newvalue);
            
        }
    }
}


</script>

<template>
    <div >

        <div class="footerCont">
            <div>Inspired By <a href="https://monkeytype.com/">monkeytype.</a></div>
        </div>
        <div class="email">
            <div class="innerEmail">

                <div>Update Announcements</div>
                <input autocomplete="email" required v-model="email" type="email" placeholder="Enter Email" />
                <button @click="SendEmail"><svg fill="#000000" height="12px" width="20px" version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" 	 viewBox="0 0 330 330" xml:space="preserve"><path id="XMLID_222_" d="M250.606,154.389l-150-149.996c-5.857-5.858-15.355-5.858-21.213,0.001c-5.857,5.858-5.857,15.355,0.001,21.213l139.393,139.39L79.393,304.394c-5.857,5.858-5.857,15.355,0.001,21.213C82.322,328.536,86.161,330,90,330s7.678-1.464,10.607-4.394l149.999-150.004c2.814-2.813,4.394-6.628,4.394-10.606C255,161.018,253.42,157.202,250.606,154.389z"/></svg></button>
            </div>

        </div>

        <div class="footerCont bottom">
            <div>Built by Edward</div>
            <a href="https://github.com/soleq1"><svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" fill="currentColor" class="bi bi-github" viewBox="0 0 16 16"><path d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27s1.36.09 2 .27c1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.01 8.01 0 0 0 16 8c0-4.42-3.58-8-8-8"/></svg></a>
        </div>
    </div>
</template>

<style>
.footerPos{
    margin-top:10px;
    /* position: relative;
    width:100%;
    bottom:8px;
 */

}
    .footerCont{
        justify-content: center;
        display: flex;
        gap:10px;
        
    }
    .bottom{
        
        margin-top:10px;
        }
    .email{
        display:flex;
        justify-content: center;
    }
    .innerEmail{
        border: solid white  2px;
        padding:5px;
        border-radius:5px;
        display:block;
    }
</style>