<script setup>
import axios from 'axios'
import { ref, onMounted } from 'vue'

// 1. Définition des colonnes (la clé 'actions' est indispensable pour le slot)
const columns = [
  { id:'id', key: 'id', label: 'ID', sortable: true },
  { id:'titre', key: 'titre', label: 'Titre' },
  { id:'description', key: 'descr', label: 'Description' },
  { id:'image', key: 'im', label: 'Image' },
  { id:'date', key: 'date', label: 'Date' },
    { id:'Demo_LinkS', key: 'demo', label: 'Demo_LinkS' },
  { id:'technologie', key: 'tech_user', label: 'Technologie' },
  { id:'GitHub', key: 'git', label: 'GitHub' }, 
  { id:'actions', key: 'actions', label: 'Actions' } 
]

const items = ref([])
const pending = ref(true)

// 2. Récupération des données depuis API Platform
const fetchProjects = async () => {
  pending.value = true
  try {
    // Note : API Platform utilise souvent le format JSON-LD
    const { data } = await axios.get('http://127.0.0.1:8000/api/Projet')
    // On cible 'hydra:member' qui contient la liste des objets
    items.value = [
  { id: 1, titre: 'Projet Test', descr: 'Ceci est un test', GitHub: 'https://github.com/user/repo', DemoLinkS:'yo',image:'yii',  tech_user: 'Nuxt/Symfony', date: '2026-03-30' }
] || data
  } catch (error) {
    console.error("Erreur lors du chargement des données", error)
  } finally {
    pending.value = false
  }
}

// 3. Fonctions d'actions
const onEdit = (row) => {
  console.log('Modifier l\'élément :', row.id)
  // Ici, tu peux ouvrir une modale ou rediriger vers une page d'édition
}

const onDelete = async (id) => {
  if (confirm('Êtes-vous sûr de vouloir supprimer cet élément ?')) {
    try {
      await axios.delete(`http://127.0.0.1:8000/api/Projet/${id}`)
      // Rafraîchir la liste après suppression
      fetchProjects()
    } catch (error) {
      console.error("Erreur de suppression", error)
    }
  }
}

onMounted(fetchProjects)
</script>
<template>
  <UContainer class="py-10">
    <UCard>
      <template #header>
        <div class="flex justify-between items-center">
          <h2 class="text-xl font-bold text-yellow-900 dark:text-white">Projets Dashboard</h2>
          <UButton icon="i-heroicons-plus" label="Nouveau Projet" color="primary" />
        </div>
      </template>

      <UTable 
        :rows="items" 
        :columns="columns" 
        :loading="pending"
      >
        <template #date-data="{ row }">
          {{ new Date(row.date).toLocaleDateString() }}
        </template>

        <template #actions-data="{ row }">
          <div class="flex gap-2">
            <UButton
              icon="i-heroicons-pencil-square"
              size="xs"
              color="blue"
              variant="ghost"
              @click="onEdit(row)"
            />
            <UButton
              icon="i-heroicons-trash"
              size="xs"
              color="red"
              variant="ghost"
              @click="onDelete(row.id)">
            Sup</UButton>
          </div>
        </template>
      </UTable>
    </UCard>
  </UContainer>
</template>