<script setup lang="ts">
import { PhoneCall } from 'lucide-vue-next';
import { Mail } from 'lucide-vue-next';
import { MapPin } from 'lucide-vue-next';


</script>

<template>
    <section id="CONTACTS"></section> <h1 class="py-5 text-center text-blue-600 text-4xl font-bold mb-4"><strong>CONTACTEZ MOI A</strong></h1>
       <div class="flex justify-between gap-7">
        <div class="text-white"><li><h2><strong><MapPin class="w-4 h-4  inline mr-2"/>ADRESSE</strong></h2>
        <p><strong>CAMEROUN-Douala (ouvert a la localisation)</strong></p>
        </li>
        </div>
        <div class="text-white"><li><h2><strong><PhoneCall class="w-4 h-4  inline mr-2"/>MOBILE</strong></h2>
            <p><strong>+237 696 848 829</strong></p>
        </li>
        </div>
        <div class="menu-link text-white">
            <li><h2><strong><Mail class="w-4 h-4  inline mr-2"/><Mail class="w-4 h-4 inline mr-2"/>EMAIL</strong></h2>
           <p><strong><a href="mailto:ndjadjavaldes@gmail.com">ndjadjavaldes@gmail.com</a></strong></p>
           </li>
        </div>
       </div>
</template>