<script setup>
import HeaderNav from '~/components/HeaderNav2.vue';
import { ref, onMounted } from 'vue'
import axios from 'axios'


const newProject = ref({
   id: null,
    Nom_V: '',
    Prenom_V: '',
    Adrs_V: '',
    Mail_V: '',
     Image_V: null,
    BP_V: '',
    Date_I_V: '',
    Demande_V: '',
    Num_V: '',
    Loisirs_V: '',
    Datenais_V: ''
})

function resetForm() {
  Object.assign(newProject.value, {
    id: null,
    Nom_V: '',
    Prenom_V: '',
    Adrs_V: '',
    Mail_V: '',
     Image_V: null,
    BP_V: '',
    Date_I_V: '',
    Demande_V: '',
    Num_V: '',
    Loisirs_V: '',
    Datenais_V: ''
  })
}


function handleFile(event) {
  const file = event.target.files[0]
  if (file) {
    newProject.value.Image_V = file
  }
}
const items = ref([])

async function handleSave() {
  try {
    if (newProject.value.Nom_V && newProject.value.Prenom_V && newProject.value.Adrs_V && newProject.value.Mail_V && newProject.value.BP_V && newProject.value.Date_I_V && newProject.value.Demande_V && newProject.value.Num_V &&  newProject.value.Loisirs_V  && newProject.value.Datenais_V) {
     if (newProject.value.Date_I_V instanceof Date) {
  newProject.value.Date_I_V = newProject.value.Date_I_V.toISOString().split('T')[0]
}

if (newProject.value.Datenais_V instanceof Date) {
  newProject.value.Datenais_V = newProject.value.Datenais_V.toISOString().split('T')[0]
}
 const data = new FormData()
      data.append('Nom_V', newProject.value.Nom_V)
      data.append('Prenom_V', newProject.value.Prenom_V)
      data.append('Adrs_V', newProject.value.Adrs_V)
      data.append('Mail_V', newProject.value.Mail_V)
      data.append('BP_V', newProject.value.BP_V)
      data.append('Date_I_V', newProject.value.Date_I_V)
      data.append('Demande_V', newProject.value.Demande_V)
      data.append('Num_V', newProject.value.Num_V)
      data.append('Loisirs_V', newProject.value.Loisirs_V)
      data.append('Datenais_V', newProject.value.Datenais_V)
      
      data.append('Image_V', newProject.value.Image_V)
      
const response = await axios.post('http://127.0.0.1:8000/api/upload/Visiteurs', data, {
  headers: { 'Content-Type': 'multipart/form-data' }
})



      items.value.push(response.data)
      console.log("Projet créé :", response.data)
 return navigateTo('/enregistrer')
      resetForm()
    } else {
      alert("Veuillez remplir tous les champs requis.")
    }
  } catch (error) {
    console.error("Erreur lors de l'enregistrement :", error)
  }
}



</script>
<template>
  <div class="bg-slate-900">
  <HeaderNav2/>
    <UContainer>
        <UPageCard  class="w-full h-full shodow-md overflow-hidden bg-gray-200  mx-auto mt-10" >
           
            <div class="text-base font-semibold leading-6 text-center px-30"><strong>
    <h1 class="text-sky-400 font-weight text-3xl text-center">   BIENVENUE DANS LA ZONE D'ENREGISTREMENT</h1>
    <h2 class="text-center font-bold text-xl text-gray PY-20">VEUILLEZ REMPLIR LES CHAMPS NECCESSAIRE POUR VOTRE IDENTIFICATION</h2>
    </strong></div>
       <form class="space-y-6">
        <div class=" md:grid-cols-2 gap-1 flex justify-between">
          <div class="flex flex-col gap-1">
          <h3 class="text-gray-500 text-lg py-3  font-bold max-w-prose" >NOM:</h3>
            <UFormGroup label="Nom">
            <UInput v-model="newProject.Nom_V" placeholder="Ex: Dupont" />
          </UFormGroup>
          <h3 class="text-gray-500 text-lg py-3  font-bold max-w-prose" >PRÉNOM:</h3>
          <UFormGroup label="Prénom">
            <UInput v-model="newProject.Prenom_V" placeholder="Ex: Jean" />
          </UFormGroup>
          <h3 class="text-gray-500 text-lg py-3  font-bold max-w-prose" >ADRESSE:</h3>
          <UFormGroup label="Email">
            <UInput v-model="newProject.Adrs_V" placeholder="Ex: 123 Rue djodjo, bonapiso" />
          </UFormGroup>
          <h3 class="text-gray-500 text-lg py-3  font-bold max-w-prose" >EMAIL:</h3>
          <UFormGroup label="Email">
            <UInput v-model="newProject.Mail_V" placeholder="Ex: jean.dupont@example.com"	type="email" required />
          </UFormGroup></div>
          <div>
          <h3 class="text-gray-500 text-lg py-3  font-bold max-w-prose" >BOITE POSTALE:</h3>
          <UFormGroup label="Boite postale">
            <UInput v-model="newProject.BP_V" placeholder="Ex: bp:12345" />
          </UFormGroup>
          <h3 class="text-gray-500 text-lg py-3  font-bold max-w-prose" >UNE PHOTO DE VOUS:</h3>
          <UFormGroup label="Email">
            <UInput 
              type="file" 
    accept="image/*" 
    @change="handleFile"
            /> </UFormGroup>
          <h3 class="text-gray-500 text-lg py-3  font-bold max-w-prose" >Date DU JOUR:</h3>
          <UFormGroup label="Date d'inscription">
            <UInput v-model="newProject.Date_I_V" type="date" />
          </UFormGroup></div>
          <div>
          <h3 class="text-gray-500 text-lg py-3  font-bold max-w-prose" >LE NOM DE PROJET:</h3>
          <UFormGroup label="Nom du projet">
            <UInput v-model="newProject.Demande_V" placeholder="Ex: application de gestion" />
          </UFormGroup>
           <h3 class="text-gray-500 text-lg py-3  font-bold max-w-prose" >NUMERO DE TELEPHONE:</h3>
          <UFormGroup label="Numero">
            <UInput v-model="newProject.Num_V" placeholder="Ex: +xxx xxx xxx xxx" type="tel"  required />
          </UFormGroup>
                <h3 class="text-gray-500 text-lg py-3  font-bold max-w-prose" >LOISIRS:</h3>
          <UFormGroup label="Loisirs" class="md:col-span-2">
            <UInput v-model="newProject.Loisirs_V" placeholder="foot, jeux, video, musique..." />
          </UFormGroup>
          <h3  class="text-gray-500 text-lg py-3  font-bold max-w-prose" >DATE DE NAISSANCE:</h3>
              <UFormGroup label="Date de naissance" class="md:col-span-2">
            <UInput v-model="newProject.Datenais_V" type="date" />
          </UFormGroup>
        </div>
        </div>
        <div class="flex justify-between">
        <div><NuxtLink to="/">
          <UButton icon="i-lucide-move-left">Retour</UButton></NuxtLink></div>
           <div>
            <UButton type="button" @click="handleSave" color="primary" icon="i-lucide-save">Soumettre</UButton>
        </div>
        </div>

      </form>
        </UPageCard>
    </UContainer>
    </div>
</template>