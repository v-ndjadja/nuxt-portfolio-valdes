<template>
   <div class="p-10">
    <div class="bg-white shadow rounded-lg p-6">
      <div class="flex justify-between items-center mb-4">
        <h2 class="text-xl font-bold text-yellow-600">Projets que vous avez recu</h2></div>
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
    <td class="px-6 py-4">{{ item.titre }}</td>
     <td class="px-6 py-4">{{ item.descr }}</td>
    <td class="px-6 py-4 text-gray-500">
        {{ item.date ? new Date(item.date).toLocaleDateString() : 'N/A' }}
    </td>
     <td class="px-6 py-4">{{ item.technologie }}</td>
  <td class="px-6 py-4 flex gap-2">{{ item.actions }}
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
<div v-if="pending" class="mt-4 text-gray-500">Chargement...</div>
    
  </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
const columns = [
  { key: 'id', label: 'ID' },
  {  key: 'titre', label: 'Titre' },
  { key: 'descr', label: 'Description' },
  {  key: 'date', label: 'Date' },
  { key: 'technologie', label: 'Technologie' },
  {  key: 'actions', label: 'Actions' } 
]

const items = ref([])
onMounted(async () => {
  try {
    const response = await axios.get('http://127.0.0.1:8000/api/demandes')
    items.value = response.data.member
    console.log("Projets chargés :", items.value)
  } catch (error) {
    console.error("Erreur lors du chargement :", error)
  }
})

async function deleteItem(id) {
  if (confirm("Voulez-vous vraiment supprimer ce projet ?")) {
    try {
      await axios.delete(`http://127.0.0.1:8000/api/demandes/${id}`)
      items.value = items.value.filter(item => item.id !== id)
      console.log(`Demande ${id} supprimée`)
    } catch (error) {
      console.error("Erreur lors de la suppression :", error)
    }
  }
}





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

</style>