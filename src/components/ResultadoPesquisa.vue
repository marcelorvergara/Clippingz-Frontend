<template>
  <b-container class="mt-4">
    <div v-if="$store.getters.getNewsResult">
      <div v-for="(items, type) in groupedItems" :key="items">
        <h5>Data: {{ type }}</h5>
        <b-table :fields="newsFields" :items="items" responsive="sm" bordered hover striped head-variant="dark">
          <template #cell(type)="{ value }">
            Data: {{ value }}
          </template>
          <template #head()="data">
            {{ data.label.toUpperCase() }}
          </template>
          <template #cell(author)="data">
            {{ data.item.author || "Desconhecida" }}
          </template>
          <template #cell(url)="data">
            <b-link target="_blank" :href="data.item.url">Link</b-link>
          </template>
          <template #cell(urlToImage)="data">
            <b-img-lazy thumbnail fluid width="140px" :src="data.item.urlToImage || image404"
                        alt="imagem da notícia"></b-img-lazy>
          </template>
        </b-table>
      </div>
    </div>
    <b-card-group columns v-if="$store.getters.getNewsResult.length === 0">
      <b-card>
        <b-card-text>
          Nenhuma notícia encontrada. Tente outra palavra-chave.
        </b-card-text>
        <b-card-text class="mt-5 text-right">
          <b-button  variant="dark" @click="$router.go(-1)">
            <span>Voltar </span><b-icon icon="backspace"></b-icon></b-button>
        </b-card-text>
        <template #footer>
          <small class="text-muted"></small>
          <b-card-text class="mt-2 text-right">
          </b-card-text>
        </template>
      </b-card>
    </b-card-group>
  </b-container>
</template>

<script>
export default {
  name: "ResultadoPesquisa",
  computed: {
    groupedItems() {
      const groups = {};
      this.$store.getters.getNewsResult.forEach(item => {
        if(groups[item.dataPublicacao]) {
          groups[item.dataPublicacao].push(item);
        } else {
          groups[item.dataPublicacao] = [item];
        }
      });
      return groups;
    }
  },
  data(){
    return {
      newsFields: [
        {key: 'titulo', label: 'Título', sortable: true},
        {key: 'url', label: 'Matéria'},
        {key: 'urlToImage', label: 'Imagem'}
      ]
    }
  }
}
</script>

<style scoped>

</style>