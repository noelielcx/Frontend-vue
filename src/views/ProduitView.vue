<template>
  <main>
    <div>
      <table>
        <caption>
          Liste des produits - page
          {{
            data.infoPage.number + 1
          }}
          /
          {{
            data.infoPage.totalPages
          }}
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
        <tr v-for="produit in data.listeProduits" :key="produit.code">
          <td>{{ produit.nom }}</td>
          <td>{{ produit.prixUnitaire }}</td>
          <td>{{ produit.unitesEnStock }}</td>
          <td>{{ produit.unitesCommandees }}</td>
        </tr>
        <tr>
          <td>
            <button @click="chargeProduits(data.links.first.href)">
              <img
                alt="Première page"
                class="logo"
                src="doublegauche.png"
                width="40"
                height="40"
              />
            </button>
          </td>
          <td>
            <button @click="chargeProduits(data.links.prev.href)">
              <img
                alt="Précédente"
                class="logo"
                src="../Downloads/fgauche.png"
                width="40"
                height="40"
              />
            </button>
          </td>
          <td>
            <button @click="chargeProduits(data.links.next.href)">
              <img
                alt="Suivante"
                class="logo"
                src="../Downloads/fdroite.png"
                width="40"
                height="40"
              />
            </button>
          </td>
          <td>
            <button @click="chargeProduits(data.links.last.href)">
              <img
                alt="Dernière page"
                class="logo"
                src="doubledroite.png"
                width="40"
                height="40"
              />
            </button>
          </td>
        </tr>
      </table>
    </div>
  </main>
</template>

<script setup>
import { reactive, onMounted } from "vue";
import { BACKEND, doAjaxRequest } from "../api";

const produitVide = {
  nom: "",
  prix: "",
  stock: "",
  commandes: "",
};

let data = reactive({
  tabProduit: { ...produitVide },
  listeProduits: [],
  links: {},
  infoPage: {},
});

function deleteEntity(entityRef) {
  doAjaxRequest(entityRef, { method: "DELETE" })
    .then(chargeCategories)
    .catch((error) => alert(error.message));
}

function chargeProduits(href) {
  doAjaxRequest(href)
    .then((json) => {
      data.listeProduits = json._embedded.produits;
      data.links = json._links;
      data.infoPage = json.page;
    })
    .catch((error) => alert(error.message));
}

function addProduit() {
  const options = {
    method: "POST",
    body: JSON.stringify(data.tabProduit),
    headers: {
      "Content-Type": "application/json",
    },
  };
  doAjaxRequest(BACKEND + "/api/produits", options)
    .then(() => {
      data.tabProduit = { ...produitVide };
      chargeProduits();
    })
    .catch((error) => alert(error.message));
}

onMounted(() =>
  chargeProduits(BACKEND + "/api/produits?size=5&sort=reference,asc&page=0")
);
</script>

<style scoped>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
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
