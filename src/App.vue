<template>
  <Loader :loading="loading" />
  <div class="card">
    <h1>Top Rated JS Repositories</h1>
    <div class="content" :ref="content">
      <Table :headers="headers" :dataSourse="repositories" />
      <VisibilityObserver v-on:became-visible="loadNext()" />
    </div>
  </div>
</template>

<script>
import Loader from "./components/Loader.vue";
import Table from "./components/Table.vue";
import VisibilityObserver from "./components/VisibilityObserver.vue";

import axios from "axios";
export default {
  name: "App",
  created() {
  },
  components: {
    Loader,
    Table,
    VisibilityObserver,
  },
  data() {
    return {
      loading: false,
      repositories: [],
      headers: [
        { prop: "name", label: "Name" },
        { prop: "url", label: "Url" },
        { prop: "owner", label: "Owner" },
        { prop: "forks", label: "Forks" },
        { prop: "open_issues", label: "Open issues" },
        { prop: "watchers", label: "Watchers" }
      ],
      page: 0,
      per_page: 20,
    };
  },
  methods: {
    loadRepos(page, per_page) {
      this.loading = true;
      return axios
        .get(
          `https://api.github.com/search/repositories?q=language:javascript&sort=stars&order=desc&page=${page}&per_page=${per_page}`
        )
        .finally(() => (this.loading = false));
    },
    loadNext() {
      this.loadRepos(this.page, this.per_page)
        .then((resp) => this.pickRepoFields(resp.data.items))
        .then((newRepos) => {
          // console.log(newRepos)
          this.repositories = [...this.repositories, ...newRepos];
        })
        .finally(() => this.page++);
    },
    pickRepoFields(repositories) {
      return repositories.map((i) => ({
        name: i.name,
        url: i.html_url,
        owner: i.owner.login,
        forks: i.forks_count,
        open_issues: i.open_issues_count,
        watchers: i.watchers,
        stars: i.stargazers_count,
        id: i.id,
      }));
    },
  },
};
</script>

<style>
body {
  margin: 0px;
}

#app {
  font-family: Roboto, Helvetica Neue, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: black;
}

h1 {
  color: #9435f2;
  text-align: left;
  font-weight: 500;
}

.content {
  width: 100%;
  overflow: scroll;
  /* height: calc(100% - 100px);  does not working properly with intersection observer*/
  height: 80vh;
}

.card {
  position: fixed;
  width: 90vw;
  height: 90vh;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.19), 0 6px 6px rgba(0, 0, 0, 0.23);
  margin: 30px;
  padding: 20px;
}

.data-table {
  overflow-y: auto;
  height: 100px;
}

.data-table thead th {
  box-shadow: 0 1px 0px rgba(0, 0, 0, 0.12);
  position: sticky;
  top: 0;
}

.data-table th,
td {
  padding-left: 24px;
  text-align: left;
  border-bottom: 1px solid rgba(0, 0, 0, 0.12);
  height: 50px;
  font-size: 14px;
  background-color: white;
}

.data-table th {
  font-size: 12px;
  font-weight: bolder;
  color: #888888;
}
</style>
