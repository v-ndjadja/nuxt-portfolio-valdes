<script setup>
import { MapPin,Phone,Mail } from 'lucide-vue-next';
</script>
<template class=" space-y-7flex flex-col">
  
    <div class="flex flex-col" >
         <Propos/>
  <Parcours/>
    <Competences/>
  <Projets/>
      <Contacts/>
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
background-color: var(--background);
</style>