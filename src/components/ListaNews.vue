<template>
  <b-container class="mt-3">
    <header
        class="mt-5"
        v-if="!$store.getters.getNewsLista.length">
      <b-card-text class="news">
        <h2>Nenhuma notícia encontrada</h2>
        <hr />
        <h4>O administrador do sistema não inseriu notícias no banco de dados para visualização dos usuários</h4>
        <span>
          Para que os usuários do site Clippingz consigam ver notícias nessa página, é necessário que o administrador
          do sistema insira notícias no sistema. Assim que a primeira notícia for inserida, essa página irá apresentar
          um resumo da(s) notícia(s) inserida(s).
        </span>
      </b-card-text>
      <b-card-text class="d-flex justify-content-center mb-3 mt-5">
        <b-img :src="no_news" alt="imagem informando que não há notícias cadastradas"></b-img>
      </b-card-text>
    </header>
    <b-carousel
        class="carousel"
        v-else
        id="carousel"
        v-model="slide"
        :interval="4000"
        fade
        controls
        indicators
        img-width="1024"
        img-height="480"
        style="text-shadow: 1px 1px 2px #333;"
        @sliding-start="onSlideStart"
        @sliding-end="onSlideEnd">
      <b-carousel-slide v-for="(news,index) in this.$store.getters.getNewsLista" :key="index"
                        :caption="news.titulo"
                        :text="news.descricao"
                        :img-src=getIagens(index)>
        <router-link
            exact :style="{ cursor: 'grab'}"
            tag="h5" class="card-title" :to="{ name:'detalhesnews', params:{news: news}}">
          <span class="vejamais">Veja Mais</span>
        </router-link>
      </b-carousel-slide>
    </b-carousel>
  </b-container>
</template>

<script>
import reading from '../assets/reading_news.png'

export default {
  name: "ListaNews",
  data(){
    return{
      slide: 0,
      sliding: null,
      imagens:'',
      no_news: reading
    }
  },
  methods:{
    //imagem aleatória para o fundo das notícias pagina prinicpal
    //utilização do index para gerar dinamicamente
    getIagens(index){
      return `https://picsum.photos/1024/480?grayscale&blur=2&random=${index}`
    },
    // eslint-disable-next-line no-unused-vars
    onSlideStart(slide) {
      this.sliding = true
    },
    // eslint-disable-next-line no-unused-vars
    onSlideEnd(slide) {
      this.sliding = false
    }
  },
  created() {
    //carregando as notícias da base de dados
    this.$store.dispatch('getNewsDB')
  }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Source+Sans+Pro&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Newsreader&display=swap');

.news{
  font-family: 'Newsreader', serif;
  color: #505050;
}
.vejamais{
  color: white;
  text-shadow: 2px 2px 4px #000000;
  font-family: 'Source Sans Pro', sans-serif;
  font-weight: bold;
}
.carousel {
  border-radius: 10px 10px 10px 10px;
  overflow: hidden;
}
</style>