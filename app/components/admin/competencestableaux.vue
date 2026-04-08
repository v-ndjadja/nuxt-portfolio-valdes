<template>
   <div class="p-10">
    <div class="bg-white shadow rounded-lg p-6">
      <div class="flex justify-between items-center mb-4">
    <UButton 
  icon="i-heroicons-plus" 
  label="Nouvelle competence" 
  color="primary" 
  @click="openCreateModal" 
/>
</div>
     <UCard  v-if="isModalOpen"  class="w-full max-w-2xl modal mx-auto"
  :ui="{ ring: '', divide: 'divide-y divide-gray-100 dark:divide-gray-800' }">
    <template #header>
      <div class="flex items-center justify-between">
        <h3 class="text-base font-semibold leading-6 text-gray-900">
          {{ isEditing ? 'Modifier la compétence' : 'Ajouter une compétence' }}
        </h3>
     </div>
    </template>
    <form @submit.prevent="handleSave" class="space-y-4">
     <div class="flex justify beetween gap-20 ">
      <div> <div><span><h2>Nom de la competences:</h2></span>
      <UFormGroup name="nom" label="nom de la competence" >
        <UInput v-model="newProject.nom" placeholder="competence..." required />
      </UFormGroup></div>
<div><span><h2>Categorie de la compeence:</h2></span>
      <UFormGroup name="catg" label="Categorie">
        <UInput v-model="newProject.catg" placeholder="Ex:back-end/Front-end" required />
        </UFormGroup></div>
<div><span><h2>Icone:</h2></span>
      <UFormGroup name="icone" label="Icone">
        <UInput v-model="newProject.icon"/>
      </UFormGroup></div></div>
      <div><span><h2>Description:</h2></span>
      <UFormGroup name="descr" label="Description">
        <UInput v-model="newProject.descr" placeholder="decrivez la competence"/>
      </UFormGroup></div>
     <div><div><span><h2>niveau de maitrise:</h2></span> 
      <UFormGroup label="Niveau" name="niveau">
          <div class="flex items-center gap-2">
            <URange v-model="newProject.niveau" :min="0" :max="100" color="red" />
            <span class="text-sm font-bold">{{ newProject.niveau }}%</span>
          </div>
        </UFormGroup></div>
      </div>
    </div>
 <div class="flex justify-end gap-3 mt-4">
        <UButton color="gray" variant="soft" @click="closeModal">Annuler</UButton>
        <UButton type="submit" color="primary">Enregistrer</UButton>
      </div>
    </form>
   
  </UCard>
      <table class="divide-y divide-gray-200 w-full max-w-2xl mx-auto Ucard-table">
        <thead class="bg-gray-50">
          <tr>
            <th v-for="col in columns" :key="col.key" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
              {{ col.label }}
            </th>
          </tr>
        </thead>
       <tbody  v-if="items.length > 0" class="bg-white divide-y divide-gray-200">
  <tr  v-for="(item, index) in items" :key="item.id">
    <td class="px-6 py-4">{{ index + 1 }}</td>
    <td class="px-6 py-4">{{ item.Nom }}</td>
    <td class="px-6 py-4">{{ item.Catg }}</td>
   <td class="px-6 py-4">{{ item.Icon }}</td>
    <td class="px-6 py-4">{{ item.Descr }}</td>
    <td class="px-6 py-4">
   <td class="border border-gray-300 px-4 py-2">
          <div class="w-full bg-gray-200 rounded-full h-4">
            <div
              class="bg-blue-500 h-4 rounded-full"
              :style="{ width: item.Niveau + '%' }"
            ></div>
          </div>
          <span class="text-sm text-gray-600">{{ item.Niveau }}%</span>
        </td>>
</td>
    
<td class="px-6 py-4 flex gap-2">{{ item.actions }}
    <UButton 
      icon="i-heroicons-pencil-square" 
      size="sm" 
      color="blue" 
      variant="soft" 
      label="Modifier"
      @click="openEditModal(item)" 
    />

    <UButton 
      icon="i-heroicons-trash" 
      size="sm" 
      color="red" 
      variant="soft" 
      label="Supprimer"
      @click="deleteProject(item.id)" 
    />
  </td>  </tr>
</tbody>
<tbody v-else class="bg-white">
  <tr>
    <td :colspan="columns.length" class="px-6 py-10 text-center text-gray-500 italic">
      Aucun projet trouvé dans la base de données Symfony.
    </td>
  </tr>
</tbody>
      </table>

      <div v-if="pending" class="mt-4 text-gray-500">Chargement...</div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
const columns = [
  { key: 'id', label: 'ID' },
  {  key: 'Nom', label: 'Titre' },
  { key: 'Catg', label: 'Categorie' },
  {  key: 'Descr', label: 'Description' },  
  {  key: 'Icon', label: 'Icone' },
    {  key: 'Niveau', label: 'Maitrise' }, 
    {  key: 'actions', label: 'Actions' }
]
const items = ref([])
onMounted(async () => {
  try {
    const response = await axios.get('http://127.0.0.1:8000/api/competences')
    items.value = response.data.member
    console.log("Projets chargés :", items.value)
  } catch (error) {
    console.error("Erreur lors du chargement :", error)
  }
})

const isModalOpen = ref(false)

function openModal() {
  isModalOpen.value = true
}

function closeModal() {
  isModalOpen.value = false
}



const newProject = ref({
 nom: '',
  catg: '',      
  descr: '',         
  icon: '',        
  niveau: 0,       // Pour le lien démo
})
 

   
    newProject.value = { 
       nom: '',
  catg: '',      // On utilise 'descr' au lieu de 'description'
  descr: '',         // Pour l'image
  icon: '',        // Pour le lien GitHub
  niveau: '',
    }
 


const isEditing = ref(false)
</script>

<style scoped>
.table-container {
  max-width: 800px;
  margin: 20px auto;
}
button {
  margin-right: 5px;
  padding: 4px 8px;
  cursor: pointer;
}
.Ucard-table{
  display:block;
  overflow-x: auto;
  white-space: nowrap;
  max-width: 100%;
}
</style>