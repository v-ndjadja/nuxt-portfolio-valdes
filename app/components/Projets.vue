<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const projets = ref([])

onMounted(async () => {
  try {
    const response = await axios.get('http://127.0.0.1:8000/api/projets')
    projets.value = response.data['hydra:member'] || []
  } catch (error) {
    console.error("Erreur lors du chargement des projets :", error)
  }
})
</script>
<template>
     <h1><section id="PROJETS" class="py-5 text-4xl text-blue-600 font-bold mb-4 text-center">PROJETS</section></h1>
  <div class="max-w-7xl mx-auto p-6">
    <h1 class="text-3xl font-bold text-center mb-10 text-slate-800">Mes Projets Réalisés</h1>

    <div v-if="items && items.length > 0" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
      
      <div v-for="item in items" :key="item.id" 
           class="group bg-white rounded-2xl shadow-md hover:shadow-2xl transition-all duration-300 transform hover:-translate-y-2 border border-gray-100 overflow-hidden">
        
        <div class="h-48 overflow-hidden bg-gray-200">
          <img v-if="item.Im" :src="'http://127.0.0.1:8000' + item.Im" 
               class="w-full h-full object-cover group-hover:scale-110 transition-transform duration-500" 
               alt="Aperçu projet">
          <div v-else class="flex items-center justify-center h-full text-gray-400">
            <i class="i-heroicons-photo text-4xl"></i>
          </div>
        </div>

        <div class="p-5">
          <div class="flex justify-between items-start mb-2">
            <h3 class="font-bold text-xl text-slate-900">{{ item.Titre }}</h3>
            <span class="px-2 py-1 bg-yellow-100 text-yellow-700 text-xs font-semibold rounded-lg">
              {{ item.Tech_user }}
            </span>
          </div>

          <p class="text-gray-600 text-sm line-clamp-3 mb-4">
            {{ item.Descr }}
          </p>

          <div class="flex justify-between items-center pt-4 border-t border-gray-50">
            <span class="text-xs text-gray-400 italic">Fait le : {{ showRawDate(item.Date) }}</span>
            <div class="flex gap-3">
              <a :href="item.Git" target="_blank" class="text-slate-800 hover:text-black">
                <UIcon name="i-heroicons-code-bracket" class="w-5 h-5" />
              </a>
              <a :href="item.Demo" target="_blank" class="text-blue-600 hover:text-blue-800">
                <UIcon name="i-heroicons-globe-alt" class="w-5 h-5" />
              </a>
            </div>
          </div>
        </div>
      </div>

    </div>

    <div v-else class="text-center py-20">
      <p class="text-gray-500 italic">Chargement des projets...</p>
    </div>
  </div>
<div><NuxtLink to="/soumettre"><UButton type="button"  color="warning" icon="i-lucide-save">Soumettre un projet</UButton></NuxtLink></div>
 <button  type="button"  color="warning" icon="i-lucide-download" @click="downloadPdf">Télécharger en PDF</button>

</template>