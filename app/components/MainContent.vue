<script setup>
import { MapPin,Phone,Mail } from 'lucide-vue-next';
</script>
<template class=" space-y-7flex flex-col">
  
    <div class="flex flex-col" >
         <Propos/>
  <Parcours/>
   <h1><section id="PROJETS" class="py-5 text-4xl font-bold mb-4 text-center">PROJETS</section></h1>
    <div>
         <div class="px-20"> <li ><h2><strong>PORTFOLIO</strong></h2></li></div>
        <p style="line-height:2"class="text-gray-300">
            J'ai développé un portfolio personnel pour présenter mes compétences et mes projets. Ce projet m'a permis de mettre en pratique mes compétences en développement web, en utilisant des technologies telles que HTML, CSS et nuxt pour créer une interface attrayante et fonctionnelle.
        </p>
    </div> 
      <section id="CONTACTS"></section> <h1 class="py-5 text-center text-4xl font-bold mb-4"><strong>CONTACTEZ MOI A</strong></h1>
       <div class="flex justify-between gap-7">
        <div><li><h2><strong><MapPin class="w-4 h-4 inline mr-2"/>ADRESSE</strong></h2>
        <p>CAMEROUN (ouvert a la localisation)</p>
        </li>
        </div>
        <div><li><h2><strong><Phone class="w-4 h-4 inline mr-2"/>MOBILE</strong></h2>
            <p><strong>+237 696 848 829</strong></p>
        </li>
        </div>
        <div class="menu-link">
            <li><h2><strong><Mail class="w-4 h-4 inline mr-2"/>EMAIL</strong></h2>
           <p><strong><a href="mailto:ndjadjavaldes@gmail.com">ndjadjavaldes@gmail.com</a></strong></p>
           </li>
        </div>
       </div>
    </div>
</template>
<style>
.menu-link{
    transition: color 0.3s ease;
    text-decoration: none;
}
.menu-link:hover{
    color:chocolate
}

</style>