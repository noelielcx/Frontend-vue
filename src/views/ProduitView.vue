<template>
  <main>
    <div>
      <!-- Un formulaire pour saisir les valeurs de la catégorie à ajouter -->
      <form @submit.prevent="ajouteProduit"></form>
    </div>
  </main>
</template>

<script setup>

function ajouteProduit() {
// Ajouter une catégorie avec les données du formulaire
const options = {
    method: "POST", // Verbe HTTP POST pour ajouter un enregistrement
    body: JSON.stringify(data.formulaireCategorie),
    headers: {
        "Content-Type": "application/json",
    },
};
</script>
