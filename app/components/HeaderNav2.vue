<script setup>
import { Github } from 'lucide-vue-next';       
</script>
<template><section id="ACCUEIL"></section>
    <header >
                <div class="flex flex-col justify-between px-10 py-2">
                <div class="flex justify-between"> 
                <div>
<h3 style=" font-size: 24px;" class="text-emerald-400"><p>salut !</p></h3>
               </div>
               <div  class=" text-teal-200 flex justify-between items-center gap-1 text-xs">
    <github/>
<a href="#https://github.com/v-ndjadja/nuxt-portfolio-valdes.git" class="menu-link">GITHUB</a>
</div>
</div>
</div>
        <div class="flex justify-between   px-10 py-2">
            <div>
              
                <p> 
                    <h1 class=" max-w-2pxl flex items-center justify-center font-bold "> 
                <strong><span class="text-4xl text-gray-300 font-bold">je suis</span> <span  class=" text-sky-400 font-weight tracking-wide text-4xl  mb-4""> NDJADJA VALDES.D</span> </strong>
                 </h1>
            </p>
           
             
            <h2>
                <p class="max-w-[300px] text-white text-2xl leading-relaxed mt-4">
        Developpement de site web concepion et realisation d'application pour but diverse.  
              </p>
            </h2>
        </div>

        <div> <img src="/logo de valdes.png " class="  flex items-center mx-auto w-64 h-64 rounded-3xl  object-cover border-4 border-black shadow-lg"></div>
        </div>
        </header>
</template>