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
     <UCard  v-if="isModalOpen" v-model="isModalOpen" class="w-full max-w-2xl mx-auto"
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
        <UButton color="gray" variant="soft" @click="isModalOpen = false">Annuler</UButton>
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
       <tbody v-if="items.length > 0" class="bg-white divide-y divide-gray-200">
  <tr v-for="item in items" :key="item.id">
    <td class="px-6 py-4">{{ item.id }}</td>
    <td class="px-6 py-4">{{ item.nom }}</td>
    <td class="px-6 py-4">{{ item.catg }}</td>
   <td class="px-6 py-4">{{ item.icon }}</td>
    <td class="px-6 py-4">{{ item.descr }}</td>
    <td class="px-6 py-4">
  <div class="flex items-center gap-2">
    <div class="w-24 bg-gray-200 rounded-full h-2">
      <div 
        class="bg-blue-600 h-2 rounded-full" 
        :style="{ width: item.niveau + '%' }"
      ></div>
    </div>
    <span class="font-bold text-blue-600">{{ item.niveau }}%</span>
  </div>
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
// 1. DÉCLARATIONS (Une seule fois !)
const columns = [
  { key: 'id', label: 'ID' },
  {  key: 'nom', label: 'Titre' },
  { key: 'catg', label: 'Categorie' },
  {  key: 'descr', label: 'Description' },  
  {  key: 'icon', label: 'Icone' },
    {  key: 'niveau', label: 'Niveau' }, 
    {  key: 'actions', label: 'Actions' }
]

// Données du tableau
const items = ref([
  {
    id: 1,
    nom: 'Projet Test BTS',
     catg: 'Nuxt 3 / Symfony',
    descr: 'Si tu vois cette ligne, le tableau fonctionne !',
    icon: 'https://example.com/demo1',
    niveau: '50',
  },
  {
     id: 2,
    nom: 'Projet Test BTS',
     catg: 'Nuxt 3 / Symfony',
    descr: 'Si tu vois cette ligne, le tableau fonctionne !',
    icon: 'https://example.com/demo1',
    niveau: '60',
   
  }
])

const pending = ref(false)

const fetchProjects = async () => {
  // Exemple : simuler un fetch
  pending.value = true
  setTimeout(() => {
    pending.value = false
    console.log('competences chargées !')
  }, 500)
}

onMounted(fetchProjects)
const newProject = ref({
 nom: '',
  catg: '',      
  descr: '',         
  icon: '',        
  niveau: 0,       // Pour le lien démo
})
 const handleAddProject = async () => {
  try {
    // 1. Appel API réel vers ton Symfony
    // Note : On utilise 'Projets' ou 'projets' selon ton entité Symfony
    await axios.post('http://127.0.0.1:8000/api/Competences', newProject.value)
    
    // 2. Mise à jour immédiate de l'affichage (pour éviter d'attendre le serveur)
    const projetAAjouter = { 
        ...newProject.value, 
        id: items.value.length + 1 
    }
    items.value.push(projetAAjouter)

    isModalOpen.value = false 
    newProject.value = { 
       nom: '',
  catg: '',      // On utilise 'descr' au lieu de 'description'
  descr: '',         // Pour l'image
  icon: '',        // Pour le lien GitHub
  niveau: '',
    }
    
    alert("competences ajouté avec succès !")
    
    // 4. Rafraîchir les données depuis le serveur pour être sûr
    fetchProjects() 

  } catch (error) {
    console.error("Erreur d'ajout", error)
    alert("Erreur lors de l'ajout. Vérifie que ton serveur Symfony est lancé et que le CORS est configuré.")
  }
}

// --- FONCTION SUPPRIMER ---
const deleteProject = async (id) => {
  // Toujours demander confirmation avant de supprimer en base de données
  if (confirm("Voulez-vous vraiment supprimer ce projet de la base de données Symfony ?")) {
    try {
      // On envoie la requête DELETE à l'API
      await axios.delete(`http://127.0.0.1:8000/api/Competences/${id}`)
      
      // On met à jour l'affichage localement pour que la ligne disparaisse de suite
      items.value = items.value.filter(p => p.id !== id)
      
      alert("Projet supprimé avec succès !")
    } catch (error) {
      console.error("Erreur lors de la suppression :", error)
      alert("Erreur : Vérifiez que le serveur Symfony est lancé et que le CORS est OK.")
    }
  }
}
 // Pour savoir si on crée ou si on modifie
const currentProjectId = ref(null)
const handleSave = async () => {
  try {
    if (isEditing.value) {
      // --- CAS MODIFICATION ---
      await axios.put(`http://127.0.0.1:8000/api/Competences/${currentProjectId.value}`, newProject.value)
      alert("Compétence mise à jour !")
    } else {
      // --- CAS AJOUT ---
      await axios.post('http://127.0.0.1:8000/api/Competences', newProject.value)
      alert("Nouvelle compétence ajoutée !")
    }

    // On ferme et on rafraîchit
    isModalOpen.value = false
    fetchProjects() 
    
  } catch (error) {
    console.error("Erreur lors de l'enregistrement", error)
  }
}

// 1. Ta variable est bien fausse par défaut
const isModalOpen = ref(false)
const isEditing = ref(false)
const openEditModal = (competence) => {
  isEditing.value = true
  currentProjectId.value = competence.id
  newProject.value = { ...competence }
  isModalOpen.value = true // La modal apparaît aussi ici
}
// 2. Fonction pour ouvrir en mode AJOUT
const openCreateModal = () => {
  isEditing.value = false
  // On vide le formulaire pour qu'il soit tout neuf
  newProject.value = {
    nom: '',
    catg: '',
    descr: '',
    icon: '',
    niveau: 0
  }
  isModalOpen.value = true // La modal apparaît grâce au v-if
}

// 3. Ta fonction pour ouvrir en mode MODIFICATION (déjà correcte)

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