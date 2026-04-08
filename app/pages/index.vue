<script setup>
</script>
<template>
      <div id="content">

    <div class="flex flex-col min-h-screen justify-between bg-slate-900">
      <div style="position: sticky ; top:0 ;z-index:100;color: var(--foregroundt);background-color: var(  --muted) ;" class="flex justify-between items-center px-10 py-2 b-border-gray-500">
            <div  class="text-white text-xs font-mono"> 
                <p style="font-size:24px" class=" flex items-start justify-center font-bold text-xl">
                    <strong>ndjadjavaldes.me <label>titre</label></strong>
                </p>
</div>
<nav > 
            <ul class=" text-white flex items-center gap-4">
                <li><a
                    href="#ACCUEIL"class="menu-link">ACCUEIL</a>

                </li>
                <li><a href="#A PROPOS"class="menu-link">A PROPOS</a>

                </li>
                <li><a href="#PARCOURS"class="menu-link">PARCOURS</a>

                </li>
                <li>
                    <a href="#COMPETENCES"class="menu-link">COMPETENCES</a>
                </li>
                <li><a href="#PROJETS"class="menu-link">PROJETS</a>

                </li>
                <li>
                    <a href="#CONTACTS"class="menu-link">CONTACT</a>
                </li>

            </ul>
        </nav>


        </div>

<div>
<HeaderNav
/>
</div>
<APPimage/>
<div class="container mx-auto py-10 px-4">
   <MainContent/>
   </div>
<div>
    <UButton  type="button"  color="warning" icon="i-lucide-download" @click="downloadPdf">Télécharger en PDF</UButton>

   </div>
   <FooterNav/>
</div>
</div>
</template>
<script setup lang="ts">
import jsPDF from 'jspdf'
import html2canvas from 'html2canvas'
function downloadPdf() {
  // Déclenche la boîte de dialogue d'impression du navigateur
  window.print();
}

</script>

<style scoped>
.menu-link{
   transition: color 0.3s ease;
   text-decoration: none;
}
.menu-link:hover{
   color: chocolate;

}
@media print {
  /* 1. On cache tout ce qui ne doit pas être sur le rapport (boutons, nav, etc.) */
  .print\:hidden, 
  button, 
  header {
    display: none !important;
  }

  /* 2. On réinitialise le corps de la page pour le PDF */
  body {
    background-color: white !important;
    margin: 0;
    padding: 0;
    -webkit-print-color-adjust: exact; /* Force l'impression des couleurs Tailwind */
    print-color-adjust: exact;
  }

  /* 3. On force le contenu à prendre toute la page proprement */
  #content {
    width: 100% !important;
    max-width: 100% !important;
    margin: 0 !important;
    padding: 20px !important;
    box-shadow: none !important; /* Enlève les ombres qui bavent au PDF */
    border: none !important;
  }

  /* 4. On évite de couper les éléments (comme tes projets) en deux entre deux pages */
  .projet-card {
    page-break-inside: avoid;
    break-inside: avoid;
    margin-bottom: 1rem;
  }
}
  </style>
