<script setup>
defineProps(['NOM','PRENOM','NIVEAU','FILIAIRE','STATUT','RESIDENCE'])
</script>
<template>
<div class="carte-identite"><h3>FICHE D'IDENTITE</h3>
<p><strong>NOM :</strong>{{NOM}}</p>
<p><strong>PRENOM :</strong>{{PRENOM}}</p>
<p><strong>NIVEAU :</strong>{{NIVEAU}}</p>
<p><strong>FILIAIRE :</strong>{{FILIAIRE}}</p>
<p><strong>STATUT :</strong>{{STATUT}}</p>
<p><strong>RESIDENCE :</strong>{{RESIDENCE}}</p>
</div>
</template>