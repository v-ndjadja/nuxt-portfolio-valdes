<script setup>
import competencestableaux from '~/components/admin/competencestableaux.vue'
import visiteurtab from '~/components/admin/visiteurtab.vue'
import projet_soumit from '~/components/admin/projet_soumit.vue'
import projet from '~/components/admin/Projet.vue'

const vueActive = ref('false')
</script>
<template>
  <div class="flex overflow-hidden">
    <aside class="w-64 bg-gray-200  border-r border-green-200 flex flex-col h-full shadow-sm  ">
      
      <div class="p-6 border-b border-gray-100">
        <h2 class="text-xl font-bold text-primary-600 flex items-center gap-2">
          <UIcon name="i-heroicons-command-line" />
          Dashboard
        </h2>
      </div>

      <div class="flex-1 overflow-y-auto p-4 space-y-2">
        <UVerticalNavigation :links="items" />
        
        <hr class="my-4 border-gray-100" />
        <div class="flex flex-col gap-3  min-h-screen">
       <div><UButton
   icon="i-lucide-briefcase-business" size="md" 
  color="primary"
   variant="solid"
  @click="vueActive = (vueActive === 'projets' ? 'none' : 'projets')">projets</UButton>
  </div>
    <div>
    <UButton
    icon="i-lucide-book" size="md"
    color="primary"
    variant="solid"
    @click="vueActive = (vueActive === 'projetsRecus' ? 'none' : 'projetsRecus')"
    > Projets recu</UButton>
   </div>
    <div>
     <UButton 
   icon="i-lucide-book-open" size="md" 
  color="primary"
   variant="solid"
  @click="vueActive = (vueActive === 'competences' ? 'none' : 'competences')">
  competences</UButton>
   </div>
   <div>
    <UButton
    icon="i-lucide-book" size="md"
    color="primary"
    variant="solid"
    @click="vueActive = (vueActive === 'visiteurs' ? 'none' : 'visiteurs')"
    > Visiteurs</UButton>
   </div>
   <div>
     <NuxtLink to="/"><UButton  
    icon="i-heroicons-arrow-left-on-rectangle" 
   size="md" 
  color="primary"
   variant="solid">Déconnexion</UButton></NuxtLink></div>
   </div>
  </div>
  <div>
     
  </div>

      
</aside>
<template>
  <UCard class="bg-slate-900  flex-1" variant="subtle">
    <Placeholder class="h-32" />
   <competencestableaux  v-if="vueActive === 'competences'" class="max-w-full w-full"/>
    <projet v-if="vueActive === 'projets'"class="w-full max-w-full" />
    <visiteurtab v-if="vueActive === 'visiteurs'" class="w-full max-w-full" />
     <projet_soumit v-if="vueActive === 'projetsRecus'" class="w-full max-w-full" />
  </UCard>
</template>

  </div>
</template>
<style>
.Ucard-table{
  display: block;
  overflow-x: auto;
  white-space: nowrap;
  max-width: 100%;
}
</style>