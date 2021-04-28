<template>
  <div class="home">
    <div class="article_container" v-if="articles && articles.length != 0">
      <div v-for="(article, index) in articles" :key="index" class="cards">
        <a :href="article.url" target="_blank">
          <h1>{{ article.title }}</h1>
          <div class="card-image">
            <img
              :src="article.urlToImage"
              alt=""
              v-if="article.urlToImage !== null"
            />
            <img src="../assets/logo.png" alt="" v-else />
          </div>
          <p>{{ article.content }}</p>
          <small>{{ article.author }}</small>
          <h5>{{ article.publishedAt | formattedDate }}</h5>
        </a>
      </div>
    </div>
    <div v-else>
      loading...
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";
import moment from "moment";

export default {
  name: "Home",
  data() {
    return {
      // info: null,
      // bpi: null,
      loading: false,
      news: null,
      articles: null,
      name: "emmanuel",
      error: false,
    };
  },
  components: {},
  // Lifecycle hooks
  mounted() {
    this.getDataFromApi();
  },
  filters: {
    capitalize(val) {
      return val.toUpperCase();
    },
    formattedDate(val) {
      return moment(val).format("llll");
    },
  },
  methods: {
    getDataFromApi() {
      this.loading = true;
      axios
        .get(
          "https://newsapi.org/v2/everything?q=tesla&from=2021-03-27&sortBy=publishedAt&apiKey=58eec6a66ff546e19495bcb37f068f71"
        )
        .then((res) => {
          this.news = res;
          this.articles = res.data.articles;
          console.log(res);
        })
        .catch((err) => {
          console.log(err);
          this.error = true;
        });
    },
  },
};
</script>

<style scoped>
.article_container {
  max-width: 700px;
  display: block;
  display: grid;
  grid-template-columns: 1fr 1fr;
  margin: auto;
}
.cards {
  background: beige;
  padding: 20px;
  margin-top: 20px;
}
.card-image img {
  width: 100%;
}
</style>
