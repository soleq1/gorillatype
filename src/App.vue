<script setup  lang="ts">
// @ts-nocheck
import _ from 'lodash'
import CoreVue from './components/CoreVue.vue';
import NavVue from './components/NavVue.vue'
import FooterVue from './components/FooterVue.vue';
import { watch } from 'vue';
import {inject} from '@vercel/analytics';



const background = localStorage.getItem('Background');
document.body.style.backgroundColor = background;
// const update = () =>{

//   setInterval(() =>{
//     const back = localStorage.getItem('Background') ;
    
//     if (back != background ){
//       document.body.style.backgroundColor = back;
      
//     }else{
//       return;
//     }
    
    
//   },1500)
// }


inject()
</script>
  
<template  >
  <div :style="{'background-color': background}"> 
     
    <NavVue :backGround="background" > </NavVue>
    <CoreVue  :backGround="background"/>
    <FooterVue :backGround="background" />
    </div>
  </template> 
  

<style  >
div{
  user-select: none;

}
</style>
