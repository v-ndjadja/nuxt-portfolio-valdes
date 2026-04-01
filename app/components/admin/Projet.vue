<script setup>
import { ref, onMounted } from 'vue'

// Définition des colonnes
const columns = [
  { key: 'id', label: 'ID' },
  { key: 'titre', label: 'Titre' },
  { key: 'descr', label: 'Description' },
  { key: 'im', label: 'Image' },
  { key: 'date', label: 'Date' },
  { key: ' demo', label: 'Demo_link' },
   { key: 'tech_user', label: 'Technologie' },
    { key: ' git', label: 'Depot_GitHub' },
     { key: 'actions', label: 'Actions' }
]


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
        <h2 class="text-xl font-bold text-yellow-600">  Dashboards projets</h2>
      </div>
       <UButton 
  icon="i-heroicons-plus" 
  label="Nouveau projet" 
  color="primary" 
  @click="openCreateModal" 
/><Ucard  v-if="isModalOpen" v-model="isModalOpen" class="w-full max-w-2xl mx-auto"
  :ui="{ ring: '', divide: 'divide-y divide-gray-100 dark:divide-gray-800' }">
  <template #header>
    <h3 class="text-lg font-semibold">Ajouter un projet</h3>
  </template>
  <template #body>
   <form @submit.prevent="handleSave" class="space-y-4">
     <div class="flex justify beetween gap-20 ">
      <div> <div><span><h2>TITRE:</h2></span>
      <UFormGroup name="nom" label="nom du projet" >
        <UInput v-model="newProject.nom" placeholder="le nom du projet" required />
      </UFormGroup></div>
<div><span><h2>Decrivez votre projet:</h2></span>
      <UFormGroup name="descr" label="Categorie">
        <UInput v-model="newProject.descr" placeholder="parlez nous du projet" required />
        </UFormGroup></div>
</div>
      <div><span><h2>Description:</h2></span>
      <UFormGroup name="descr" label="Description">
        <UInput v-model="newProject.descr" placeholder="decrivez la competence"/>
      </UFormGroup></div>
      <div><span><h2>Images:</h2></span>
      <UFormGroup name="image" label="Description">
        <UInput v-model="newProject.im" placeholder="a quoi ressemble le projet"/>
      </UFormGroup></div>
       <div><span><h2>Date:</h2></span>
      <UFormGroup name="date" label="Date ">
        <UInput v-model="newProject.date" type="date"/>
      </UFormGroup><>/div>
       <div><span><h2>Demo_link:</h2></span>
      <UFormGroup name="demo" label="Demo Link">
        <UInput v-model="newProject.demo" placeholder="Lien de la démo" type="url" required/>
      </UFormGroup></div>
      <div><span><h2>Technologie:</h2></span>
      <UFormGroup name="tech_user" label="Technologie ">
        <UInput v-model="newProject.tech_user"/>
      </UFormGroup></div>
       <div><span><h2>Depot GitHub:</h2></span>
      <UFormGroup name="git" label="Depot GitHub">
        <UInput v-model="newProject.git" placeholder="Lien du dépôt GitHub" type="url" required/>
      </UFormGroup></div>
    </div>
    </div>
 <div class="flex justify-end gap-3 mt-4">
        <UButton color="gray" variant="soft" @click="isModalOpen = false">Annuler</UButton>
        <UButton type="submit" color="primary">Enregistrer</UButton>
      </div>
    </form>
  </template>
</Ucard>

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
            <td class="px-6 py-4">{{ item.Titre}}</td>
            <td class="px-6 py-4">{{ item.descr }}</td>
            <td class="px-6 py-4">{{ item.im }}</td>
            <td class="px-6 py-4">{{ item.date }}</td>
            <td class="px-6 py-4">{{ item.demo }}</td>
            <td class="px-6 py-4">{{ item.tech_user }}</td>
            <td class="px-6 py-4">{{ item.git}}</td>
            <td class="px-6 py-4 flex gap-2">{{ item.actions }}<UButton 
      icon="i-heroicons-pencil-square" 
      size="sm" 
      color="blue" 
      variant="secondary" 
      label="Modifier"
      @click="openEditModal(item)" 
    />

    <UButton 
      icon="i-heroicons-trash" 
      size="sm" 
      color="warning" 
      variant="soft" 
      label="Supprimer"
      @click="deleteProject(item.id)" 
    />
            </td>
          </tr>
        </tbody>
      </table>

      <div v-if="pending" class="mt-4 text-gray-500">Chargement...</div>
    </div>
  </div>
</template>