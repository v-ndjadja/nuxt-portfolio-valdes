<script setup>
import HeaderNav from '~/components/HeaderNav2.vue';
import { ref, onMounted } from 'vue'
import axios from 'axios'

const newProject = ref({
  titre: '',
  descr: '',
  date: '',
  technologie: ''
})



const items = ref([])

async function handleSave() {
  try {
    if (newProject.value.titre && newProject.value.descr && newProject.value.date && newProject.value.technologie) {
      // formatage de la date si besoin
      if (newProject.value.date instanceof Date) {
        newProject.value.date = newProject.value.date.toISOString().split('T')[0]
      }

      const response = await axios.post(
        'http://127.0.0.1:8000/api/demandes',
        newProject.value,
        { headers: { 'Content-Type': 'application/ld+json' } }
      )

      items.value.push(response.data)
      console.log("Projet créé :", response.data)

      resetForm()
    } else {
      alert("Veuillez remplir tous les champs requis.")
    }
  } catch (error) {
    console.error("Erreur lors de l'enregistrement :", error)
  }
}

function resetForm() {
  Object.assign(newProject.value, {
    id: null,
    titre: '',
    descr: '',
    date: '',
    technologie: '',
  })
}
</script>
<template>
    <div class="bg-slate-900">
    <div><HeaderNav2></HeaderNav2></div>
    
    <UContainer><UPageCard class="w-full h-90 shodow-md overflow-hidden bg-gray-200  mx-auto mt-10">
      
            <div class="text-base font-semibold leading-6 text-center px-30"><strong>
    <h1 class="text-sky-400 font-weight text-3xl text-center">   BIENVENUE DANS LA ZONE D'ENREGISTREMENT DES PROJETS</h1>
    <h2 class="text-center font-bold text-xl text-gray PY-20">VEUILLEZ REMPLIR LES CHAMPS NECCESSAIRE POUR L'EVALUATION DE VOTRE PROJET</h2>
    </strong></div>
      
    <UForm class="flex justify between gap-10">
         <div>
        <h3 class=" text-gray-500 text-lg py-3  font-bold">Entrez le titre enumerer dans la demande precedente:</h3>
        <UFormGrroup label="Titre du projet">
            <UInput v-model="newProject.titre"  placeholder="Entrez le titre de votre projet" required />
        </UFormGrroup>
        <h3 class=" text-gray-500 text-lg py-3  font-bold">veuillez decrire brillevement ce que vous souhaitez mettre sur pied</h3>
        <UFormGroup label="Description du projet">
            <UTextarea  v-model="newProject.descr" placeholder="Entrez une description détaillée de votre projet" required/>
        </UFormGroup>
    </div>
        <div>
        <h3 class=" text-gray-500 text-lg py-3  font-bold">Date de ce jour</h3>
        <UFormGroup label="Date de réalisation">
            <UInput v-model="newProject.date" type="date" required />
        </UFormGroup>
        <h3 class="text-gray-500 text-lg py-3  font-bold max-w-prose">Avez vous des preferences de technologie ou langages si oui entrez les si non laissz vide:</h3>
        <UFormGroup label="Technologies utilisées">
        <UInput v-model="newProject.technologie" placeholder="Ex:nuxt,java, symfony" />   
        </UFormGroup>
    </div>
        <div><NuxtLink to="/">
          <UButton icon="i-lucide-move-left">Retour</UButton></NuxtLink></div>
           <div>
            <UButton type="button" @click="handleSave" color="primary" icon="i-lucide-save">Soumettre</UButton>
        </div>
        </UForm>
      
    </UPageCard></UContainer>
    </div>
</template>