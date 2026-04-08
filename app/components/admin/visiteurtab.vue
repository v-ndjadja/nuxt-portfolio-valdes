<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
const columns = [
  { key: 'id', label: 'ID' },
  { key: 'Nom_V', label: 'Nom' },
  { key: 'Prenom_V', label: 'Prenom' },
  { key: 'Adrs_V', label: 'Adresse' },
  { key: 'Mail_V', label: 'E-mail' },
  { key: ' Bp_V', label: 'Boite Postal' },
   { key: 'Image_V', label: 'Image' },
    { key: ' Date_I_V', label: 'Date inscription' },
     { key: ' Damande_V', label: 'Vous desirez' },
      { key: '  Loisirs_V', label: 'Loisirs' },
      { key: '  Datenais_V', label: 'Date de naissance' },
       { key: '  action', label: 'Action' }
]

function showRawDate(value) {
  if (!value || value.trim() === '') return 'N/A'
  const date = new Date(value)
  return isNaN(date.getTime()) ? 'Date invalide' : date.toLocaleDateString('fr-FR')
}



const items = ref([])
onMounted(async () => {
  try {
    const response = await axios.get('http://127.0.0.1:8000/api/visiteurs')
    items.value = response.data.member
    console.log("Projets chargés :", items.value)
  } catch (error) {
    console.error("Erreur lors du chargement :", error)
  }
})
async function deleteItem(id) {
  if (confirm("Voulez-vous vraiment supprimer ce projet ?")) {
    try {
      await axios.delete(`http://127.0.0.1:8000/api/visiteurs/${id}`)
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
        <tbody  v-if="items.length > 0" class="bg-white divide-y divide-gray-200">
  <tr  v-for="(item, index) in items" :key="item.id">
            <td class="px-6 py-4">{{ index + 1 }}</td>
            <td class="px-6 py-4">{{ item.Nom_V }}</td>
            <td class="px-6 py-4">{{ item.Prenom_V }}</td>
            <td class="px-6 py-4">{{ item.Adrs_V }}</td>
            <td class="px-6 py-4">{{ item.Mail_V }}</td>
            <td class="px-6 py-4">{{ item.Bp_V }}</td>
           <td class="px-6 py-4">
  <img
    :src="`http://127.0.0.1:8000${item.Image_V}`"
    alt="Visiteur"
    style="width:80px; height:80px; object-fit:cover; border-radius:8px;"
  />
</td>

          <td class="px-6 py-4 text-gray-500">
 {{ showRawDate(item.Date_I_V) }}
    </td>
            <td class="px-6 py-4">{{ item.Damande_V }}</td>
            <td class="px-6 py-4">{{ item.Loisirs_V }}</td>
           <td class="px-6 py-4 text-gray-500">
       <td>{{ showRawDate(item.Datenais_V) }}</td>
    </td>
            <td class="px-6 py-4 flex gap-2">{{ item.action }}
              <button class=" px-2 py-1 bg-red-200 text-red-800 rounded hover:bg-red-300" @click="deleteItem(item.id)">Supprimer</button>
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

      <div v-if="pending" class="mt-4 text-gray-500">Chargement...</div>
    </div>
  </div>
</template>