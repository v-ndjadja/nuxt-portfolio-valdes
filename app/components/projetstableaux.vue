<script setup lang="ts">
import{BookCopy} from 'lucide-vue-next';
import type { TableColumn } from '@nuxt/ui'
import type { TableMeta, Row } from '@tanstack/vue-table'
const  columns = ref([
  { accessorKey: 'ID', header: 'ID' },
  { accessorKey: 'TITRE', header: 'TITRE' },
  { accessorKey: 'DESCRIPTION', header: 'DESCRIPTION' },
   { accessorKey: 'DATE_DE_CREATION', header: 'DATE DE CREATION' },
    { accessorKey: 'IMAGE', header: 'IMAGE' },
     { accessorKey: 'NOTTATION', header: 'NOTTATION' },
       { accessorKey: 'DEMOLIVE', header: 'DEMOLIVE' },
        { accessorKey: 'REFERENCE', header: 'REFERENCE' },
         { accessorKey: 'ACTION', header: 'ACTION' },
])
const skills = ref([
  { ID: 1, TITRE: 'Java', DATE_DE_CREATION: '10/03/2023', NOTTATION: 'BONNE', DEMOLIVE: 'VBBVBV', REFERENCE: 'DHI',  },
 
])
// 3. La fonction de suppression
const supprimerSkill = (id: number) => {
  if (confirm("Supprimer cette compétence ?")) {
    skills.value = skills.value.filter(s => s.ID !== id)
  }
}
</script>

<template>
 <span><h1 class="text-center font-bold text-3xl underline text-yellow-400">GERER VOS PROJETS ici !<BookCopy/></h1></span>
  <UTable  
    :columns="columns"
    :ui="{
      td: 'text-yellow-400 font-medium text-xl border-b border-slate-800', 
      th: 'text-yellow-500 font-bold uppercase text-xs tracking-widest py-3 border-b border-slate-700'
    }"
  >
  <template #cell-ACTION="{ row }">
        <div class="flex gap-2">
          <UButton 
            color="primary" 
            variant="solid" 
            size="xs"
          >
            Modifier
          </UButton>

          <UButton 
            color="warning" 
            variant="solid" 
            size="xs"
           
          >
            Supprimer
          </UButton>
        </div>
      </template>
  </UTable>
</template>