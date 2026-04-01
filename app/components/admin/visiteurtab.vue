<script setup>
import { ref, onMounted } from 'vue'

// Définition des colonnes
const columns = [
  { key: 'id', label: 'ID' },
  { key: 'nom', label: 'Nom' },
  { key: 'prenom', label: 'Prenom' },
  { key: 'adrs_v', label: 'Adresse' },
  { key: 'mail_v', label: 'E-mail' },
  { key: ' bp_v', label: 'Boite Postal' },
   { key: 'image_v', label: 'Image' },
    { key: ' date_i_v', label: 'Date inscription' },
     { key: ' damande_v', label: 'Vous desirez' },
      { key: '  loisirs_v', label: 'Loisirs' },
      { key: '  datenais_v', label: 'Date de naissance' },
       { key: '  action', label: 'Action' }
]

// Données du tableau
const items = ref([
  {
          id:1,
           nom:'alie', 
           prenom:'igor', 
           adrs_v:'douala', 
          mail_v:'alieigor@gmail.com', 
           bp_v:'3455', 
           image_v :'',
           date_i_v :'10/03/2023',
            damande_v :'une application',
          loisirs_v :'jeux video',
          datenais_v:'20/05/2007'
    
  },
  {
      id:1,
           nom:'alie', 
           prenom:'ester', 
           adrs_v:'douala', 
          mail_v:'alieester@gmail.com', 
           bp_v:'3455', 
           image_v :'',
           date_i_v :'10/03/2024',
            damande_v :'une application',
          loisirs_v :'jeux ',
          datenais_v:'20/05/2005'
  }
])

const pending = ref(false)

const fetchProjects = async () => {
  // Exemple : simuler un fetch
  pending.value = true
  setTimeout(() => {
    pending.value = false
    console.log('votre liste est chargés !')
  }, 500)
}

onMounted(fetchProjects)
</script>

<template>
  <div class="p-10">
    <div class="bg-white shadow rounded-lg p-6">
      <div class="flex justify-between items-center mb-4">
        <h2 class="text-xl font-bold text-yellow-600">apercu des visiteurs</h2>
      </div>

      <table class="min-w-full divide-y divide-gray-200 Ucard-table">
        <thead class="bg-gray-50">
          <tr>
            <th v-for="col in columns" :key="col.key" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
              {{ col.label }}
            </th>
          </tr>
        </thead>
        <tbody class="bg-white divide-y divide-gray-200">
          <tr v-for="item in items" :key="item.id">
            <td class="px-6 py-4">{{ item.id }}</td>
            <td class="px-6 py-4">{{ item.nom }}</td>
            <td class="px-6 py-4">{{ item.prenom }}</td>
            <td class="px-6 py-4">{{ item.adrs_v }}</td>
            <td class="px-6 py-4">{{ item.mail_v }}</td>
            <td class="px-6 py-4">{{ item.bp_v }}</td>
            <td class="px-6 py-4">{{ item.image_v }}</td>
            <td class="px-6 py-4">{{ item.date_i_v }}</td>
            <td class="px-6 py-4">{{ item.damande_v }}</td>
            <td class="px-6 py-4">{{ item.loisirs_v }}</td>
            <td class="px-6 py-4">{{ item.datenais_v }}</td>
            <td class="px-6 py-4 flex gap-2">{{ item.action }}
              <button class="px-2 py-1 bg-red-200 text-red-800 rounded hover:bg-red-300">Supprimer</button>
            </td>
          </tr>
        </tbody>
      </table>

      <div v-if="pending" class="mt-4 text-gray-500">Chargement...</div>
    </div>
  </div>
</template>