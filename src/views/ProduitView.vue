<template>
  <main>
    <div>
      <table>
        <caption>
          Liste des produits
        </caption>
        <tr>
          <th>Nom</th>
          <th>Prix</th>
          <th>Stock</th>
          <th>Commandés</th>
        </tr>
        <!-- Si le tableau des catégories est vide -->
        <tr v-if="!data.listeProduits">
          <td colspan="4">Veuillez patienter, chargement des produits...</td>
        </tr>
        <!-- Si le tableau des catégories n'est pas vide -->
        <tr v-for="produit in data.listeProduits" :key="produit.nom">
          <td>{{ produit.nom }}</td>
          <td>{{ produit.prix }}</td>
          <td>{{ produit.stock }}</td>
          <td>
            <button @click="deleteEntity(produit._links.self.href)">
              Supprimer
            </button>
          </td>
        </tr>
      </table>
    </div>
  </main>
</template>

<script setup>
function deleteEntity(entityRef) {
  doAjaxRequest(entityRef, { method: "DELETE" });
  //   .then(chargeCategories)
  //  .catch((error) => alert(error.message));
}
</script>

<style scoped>
td,
th {
  border: 1px solid #ddd;
  padding: 8px;
}

th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #232623;
  color: rgb(255, 255, 255);
}
</style>
