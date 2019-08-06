<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <button @click="getPageData(2)">取得該頁資料</button>
    <!-- <ul v-for="(entry, index) in allEntry" :key="entry.index"> -->
    <ul v-for="(entry, index) in pageEntry" :key="entry.index">
      <li>
        <a>{{index}}.{{entry.youtubeId}}, {{entry.Object}}, {{entry.filename}}</a>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";
import VueAxios from "vue-axios";

export default {
  name: "HelloWorld",
  props: {
    msg: String
  },

  data: function() {
    return {
      allEntry: [],

      currentPage: 0,
      perPage: 10,
      pageEntry: [],
      totalItems: 0
    };
  },
  methods: {
    getPageData: function(currentPage = 1) {
      let self = this;
      let totalEntry = self.allEntry;
      console.log("In getPageData");
      console.log("perPage =", self.perPage);
      console.log(totalEntry);
      console.log(totalEntry.slice(0, self.perPage));
      self.pageEntry = totalEntry.slice(
        self.perPage * (currentPage - 1),
        self.perPage * currentPage
      );
      console.log(self.pageEntry);
    }
  },
  created: function() {
    let self = this;
    axios({
      methods: "get",
      url: "http://140.96.0.34:50013/dataset/queryTrainYoloTag/04jm7VfInbo"
    }).then(response => {
      //self.data = response.data;
      console.log(response.data.data);
      //console.log(response.data.data.length);
      var resdata = response.data.data;
      self.allEntry = resdata;
      self.totalItems = response.data.data.length;
      console.log(self.totalItems);
      console.log(self.allEntry);
    });
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
