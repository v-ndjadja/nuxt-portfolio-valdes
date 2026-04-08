<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import { reactive } from 'vue'
const columns = [
  { key: 'id', label: 'ID' },
  { key: 'Titre', label: 'Titre' },
  { key: 'Descr', label: 'Description' },
  { key: 'Im', label: 'Image' },
  { key: 'Date', label: 'Date' },
  { key: 'Demo', label: 'Demo_link' },
   { key: 'Tech_user', label: 'Technologie' },
    { key: 'Git', label: 'Depot_GitHub' },
     { key: 'actions', label: 'Actions' }
]
const pending = ref(false)
const items = ref([])
const vueActive = ref('none')
const newProject = reactive({
  Titre: '',
  Descr: '',
  Im: '',
  Date: '',
  Demo: '',
  Tech_user: '',
  Git: ''
})
function onFileChange(event) {
  const file = event.target.files[0]
  if (file) {
    newProject.Im = file
  }
}


function toggleCard() {
  vueActive.value = (vueActive.value === 'card' ? 'none' : 'card')
}

function closeCard() {
  vueActive.value = 'none'
}
function resetForm() {
  Object.assign(newProject, {
    id: null,
    Titre: '',
    Descr: '',
    Im: '',
    Date: '',
    Demo: '',
    Tech_user: '',
    Git: ''
  })
}
function editItem(item) {
  Object.assign(newProject, item)
  vueActive.value = 'card'
}
async function handleSave() {
  try {
    const data = new FormData()
    data.append('Titre', newProject.Titre)
    data.append('Descr', newProject.Descr)
    data.append('Date', newProject.Date instanceof Date 
  ? newProject.Date.toISOString().split('T')[0] 
  : newProject.Date
)
    data.append('Demo', newProject.Demo)
    data.append('Tech_user', newProject.Tech_user)
    data.append('Git', newProject.Git)

    if (newProject.Im) {
      data.append('Im', newProject.Im) 
    }
    let response
    if (newProject.id) {
      response = await axios.put(
        `http://127.0.0.1:8000/api/projets/${newProject.id}`,
        data,
        { headers: { 'Content-Type': 'multipart/form-data' } }
      )
      const index = items.value.findIndex(p => p.id === newProject.id)
      if (index !== -1) items.value[index] = response.data
    } else {
      response = await axios.post(
        'http://127.0.0.1:8000/api/upload/projets',
        data,
        { headers: { 'Content-Type': 'multipart/form-data' } }
      )
      items.value.push(response.data)
    }
    closeCard()
    resetForm()
  } catch (error) {
    console.error("Erreur lors de l'enregistrement :", error)
  }
}
function showRawDate(value) {
  if (!value || value.trim() === '') return 'N/A'
  const date = new Date(value)
  return isNaN(date.getTime()) ? 'Date invalide' : date.toLocaleDateString('fr-FR')
}
onMounted(async () => {
  try {
  const response = await axios.get('http://127.0.0.1:8000/api/projets')
      items.value = response.data.member || response.data || []
      console.log("Projets chargés :", items.value)
  } catch (error) {
    console.error("Erreur lors du chargement :", error)
  }
})
async function deleteItem(id) {
  if (confirm("Voulez-vous vraiment supprimer ce projet ?")) {
    try {
      await axios.delete(`http://127.0.0.1:8000/api/projets/${id}`)
      items.value = items.value.filter(item => item.id !== id)
      console.log(`Demande ${id} supprimée`)
    } catch (error) {
      console.error("Erreur lors de la suppression :", error)
    }
  }
}
</script>

<template>
  <div class="p-10">
    <div class="bg-white shadow rounded-lg p-6">
      <div class="flex justify-between items-center mb-4">
        <h2 class="text-xl font-bold text-yellow-600">  Dashboards projets</h2>
      </div>
       <UButton 
  icon="i-heroicons-plus" 
  label="Nouveau projet" 
  color="primary" 
  @click="toggleCard"
  />
  <UCard v-if="vueActive === 'card'" class="max-w-4xl shadow-xl border-t-4 border-yellow-500">
      <template #header>
   <h3 class="text-lg font-bold text-slate-700">
            {{ newProject.id ? 'Modifier le projet' : 'Nouveau projet' }}
          </h3>
      </template>
   <form class="space-y-6">
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6 flex justify-between">
          <div class="flex flex-col gap-3">
            <h3 class=" text-gray-500 text-lg py-1  font-bold">Nom du projet:</h3> 
          <UFormGroup label="Titre du projet">
            <UInput v-model="newProject.Titre" placeholder="Ex: Application Tontine" />
          </UFormGroup>
           <h3 class=" text-gray-500 text-lg py-1  font-bold">Description du projet:</h3>
          <UFormGroup label="Description du projet" class="md:col-span-2">
            <UTextarea v-model="newProject.Descr" placeholder="Décrivez brièvement le projet..." />
          </UFormGroup>
               <h3 class=" text-gray-500 text-lg py-1  font-bold">Image du projet:</h3>
           <UFormGroup label="Lien de l'image" class="md:col-span-2">
            <UInput v-model="newProject.Im" placeholder="image" icon="i-heroicons-photo" type="file" @change="onFileChange"  />
          </UFormGroup>
        </div>
        <div class="flex flex-col gap-2">
           <h3 class=" text-gray-500 text-lg py-1 font-bold">Date de realisation:</h3>
          <UFormGroup label="Date de réalisation">
            <UInput v-model="newProject.Date" type="date" />
          </UFormGroup>
          <h3 class=" text-gray-500 text-lg py-1  font-bold">Lien vers la Demo:</h3>
          <UFormGroup label="Lien Démo (Live)">
            <UInput v-model="newProject.Demo" placeholder="https://..." icon="i-heroicons-link" type="url" />
          </UFormGroup>
           <h3 class=" text-gray-500 text-lg py-1  font-bold">Technologie utilisée:</h3>
           <UFormGroup label="Catégorie / Technologie">
            <UInput v-model="newProject.Tech_user" placeholder="Ex: Nuxt & Symfony" />
          </UFormGroup>
 <h3 class=" text-gray-500 text-lg py-1  font-bold">Lien vers le depot GiHub:</h3>
          <UFormGroup label="Dépôt GitHub" class="md:col-span-2">
            <UInput v-model="newProject.Git" placeholder="Lien du code source" icon="i-heroicons-code-bracket" type="url"/>
          </UFormGroup></div>

        </div>

        <div class="flex justify-end gap-3 pt-4 border-t">
          <UButton color="gray" variant="ghost" icon="i-lucide-x-circle" label="Annuler" @click="vueActive = 'none'" />
          <UButton type="button" @click="handleSave" color="black" label="Enregistrer" icon="i-lucide-save" />
        </div>
      </form>
    </UCard>

    
      <table  class="divide-y divide-gray-200 w-full max-w-2xl mx-auto Ucard-table">
        <thead class="bg-gray-50">
          <tr>
            <th v-for="col in columns" :key="col.key" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
              {{ col.label }}
            </th>
          </tr>
        </thead>
        <tbody  v-if="items && items.length > 0" class="bg-white divide-y divide-gray-200">
          <tr v-for="(item, index) in items" :key="item.id">
            <td class="px-6 py-4">{{ index + 1 }}</td>
            <td class="px-6 py-4">{{ item.Titre}}</td>
            <td class="px-6 py-4">{{ item.Descr }}</td>
            <td class="px-6 py-4"> <img
    :src="`http://127.0.0.1:8000${item.Im_V}`"
    alt="Projet"
    style="width:80px; height:80px; object-fit:cover; border-radius:8px;"
  /></td>
           <td class="px-6 py-4 text-gray-500">
        {{ showRawDate(item.Date) }}
    </td>
            <td class="px-6 py-4">{{ item.Demo }}</td>
            <td class="px-6 py-4">{{ item.Tech_user }}</td>
            <td class="px-6 py-4">{{ item.Git}}</td>
            <td class="px-6 py-4 flex gap-2">{{ item.actions }}<UButton 
      icon="i-heroicons-pencil-square" 
      size="sm" 
      color="blue" 
      variant="secondary" 
      label="Modifier"
         @click="editItem(item)" 

 
    />

    <UButton 
      icon="i-heroicons-trash" 
      size="sm" 
      color="warning" 
      variant="soft" 
      label="Supprimer"
       @click="deleteItem(item.id)"  
       class=" px-2 py-1 bg-red-200 text-red-800 rounded hover:bg-red-300"
    />
            </td>
          </tr>
        </tbody>
        <tbody v-else class="bg-white">
  <tr>
    <td :colspan="columns.length" class="px-6 py-10 text-center text-gray-500 italic">
      Aucun projet trouvé dans la base de données Symfony.
    </td>
  </tr>
</tbody>
      </table>
</div>
      <div v-if="pending" class="mt-4 text-gray-500">Chargement...</div>
    </div>
</template>