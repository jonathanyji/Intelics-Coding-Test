<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col">
        <h1 class="heading">Latest Commits</h1>
      </div>
    </div>
    <div class="row justify-content-end">
      <div class="col-2">
        <button type="button" class="btn btn-primary" v-on:click="toggleCommits">{{ commitBtn }}</button>
      </div>
    </div>
    <div class="row">
      <div class="col d-flex align-items-center">
        <input style="width: 50px" type="number" v-model="number">
        <button type="button" class="btn btn-primary ml-3" v-on:click="showNumber">Show number</button>
        <button type="button" class="btn btn-primary ml-3" v-on:click="reset">Reset</button>
      </div>
    </div>
    <ul>
      <template v-for="{ html_url, sha, author, commit } in commits" :key="html_url">
        <li>
          <div class="row mt-3">
            <div class="col">
              <span>
                <a :href="html_url" target="_blank" class="commit">{{ sha.substring(0, 8) }}</a>
              </span>
              By<i class="fa fa-fw fa-user"></i>
              <span class="font-weight-bold">
                <a :href="author.html_url" target="_blank" class="author">{{ commit.author.name }}</a>
              </span>
              at
              <span class="font-weight-bold">
                {{ formatDate(commit.author.date) }}
              </span>
            </div>
          </div>
          <div class="row mb-3">
            <div class="col">
              <span v-show="show" class="commit-message">
                {{ commit.message }}
              </span>
            </div>
          </div>
          <hr />
        </li>
      </template>
    </ul>
  </div>
</template>

<script>
import moment from "moment";

export default {
  //name: 'component-name',
  //parent: '',
  components: {},
  directives: {},
  filters: {},
  mixins: [],
  props: {},
  data: function () {
    return {
      API_URL:
        "https://api.github.com/repos/vuejs/core/commits?per_page=10&sha=main",
      commits: null,
      tmpCommits: null,
      show: true,
      commitBtn: "Show Commit",
      number: 0,
    };
  },
  computed: {},
  watch: {},
  beforeCreate: function () {
  },
  created: function () {
    this.fetchData();
    this.checkButton();
  },
  beforeMount: function () {
  },
  mounted: function () {
  },
  beforeUpdate: function () {
  },
  updated: function () {
  },
  activated: function () {
  },
  deactivated: function () {
  },
  beforeDestroy: function () {
  },
  destroyed: function () {
  },
  methods: {
    fetchData: async function () {
      this.commits = await (await fetch(this.API_URL)).json();
      this.tmpCommits = this.commits;
    },
    truncate: function (text) {
      const newlineIdx = text.indexOf("\n");
      return newlineIdx > 0 ? text.slice(0, newlineIdx) : text;
    },
    formatDate: function (date) {
      return moment(date).format('MM/DD/YYYY h:mm:ss a');
    },
    toggleCommits: function () {
      this.show = !this.show;
      this.checkButton();
    },
    checkButton() {
      this.show === true ? this.commitBtn = "Hide Commit" : this.commitBtn = "Show Commit";
    },
    showNumber() {
      let array = [];
      for (let i = 0; i < this.number; i++) {
        array.push(this.commits[i]);
      }
      this.commits = array;
    },
    reset() {
      this.commits = this.tmpCommits;
      this.number = 0;
    }

  },
};
</script>

<style lang="scss" scoped>
ul {
  list-style-type: none;
  padding: 0;
  margin: 20px;
}

.commit {
  color: rgb(76, 187, 23);
}

.author {
  color: rgb(0, 128, 0);
}

.heading {
  color: rgb(76, 187, 23);
}
</style>
