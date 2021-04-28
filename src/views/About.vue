<template>
  <div class="about">
    <p>{{ name | capitalize }}</p>
    <div class="container p-6" v-if="cases && cases.length != 0">
      <div class="row">
        <div
          class="col-3 card m-3"
          style="width: 18rem;"
          v-for="caseVal in cases"
          :key="caseVal.id"
        >
          <div class="card-body">
            <h5 class="card-title">
              {{ caseVal.All.country }}
            </h5>
            <h6 class="card-subtitle mb-2 text-muted">
              <b>Continent:</b> {{ caseVal.All.continent }}
            </h6>
            <h6 class="card-subtitle mb-2 text-muted">
              <b>Population:</b> {{ caseVal.All.population }}
            </h6>
            <h6 class="card-subtitle mb-2 text-muted">
              <b>Infected:</b> {{ caseVal.All.confirmed }}
            </h6>
            <h6 class="card-subtitle mb-2 text-muted">
              <b>Deaths:</b> {{ caseVal.All.deaths }}
            </h6>
            <h6 class="card-subtitle mb-2 text-muted">
              <b>Recoveries:</b> {{ caseVal.All.recovered }}
            </h6>
            <h6 class="card-subtitle mb-2 text-muted">
              <b>Last updated:</b> {{ caseVal.All.updated | formattedDate }}
            </h6>
          </div>
        </div>
      </div>
    </div>
    <div v-else>
      loading...
    </div>
  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";

export default {
  name: "About",
  data() {
    return {
      loading: false,
      cases: null,
      error: false,
      name: "emmanuel",
    };
  },
  mounted() {
    this.getCovidData();
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
    getCovidData() {
      this.loading = true;
      axios
        .get("https://covid-api.mmediagroup.fr/v1/cases")
        .then((res) => (this.cases = res.data))
        .catch((err) => {
          console.log(err);
          this.error = true;
        });
    },
  },
};
</script>

<style scoped>
.container {
  margin-right: 150px;
}
.card {
  border: 1.5px solid #42b983;
}
.card-body h5 {
  text-align: center;
  border: 1.5px solid #42b983;
  color: #42b983;
}
.card-body h6 {
  padding: 8px;
  text-align: left;
  font: bolder;
  font-weight: bolder;
  /*  */
  font-family: Quicksand;
  font-size: 16px;
}

.card:hover {
  background: #c0c0c0c0;
  cursor: pointer;
}
</style>
