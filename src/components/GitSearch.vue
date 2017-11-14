<template>
  <div class="GitSearch">
    <div class="container">
      <div class="row">
      <div class="col-xs-8 col-xs-offset-2">
        <form class="form-inline">

          <div class="checkbox">
            Sort By:
            <label><input type="checkbox" v-model="stars"> Stars</label>
            <label><input type="checkbox" v-model="forks"> Forks</label>
            <label><input type="checkbox" v-model="activity"> Activity</label>
          </div>

          <div class="input-group">
            <input type="text" class="form-control" placeholder="Search" v-model="key">
            <div class="input-group-btn">
              <button class="btn btn-default" type="button" v-on:click="searchKey(key)">
                <i class="glyphicon glyphicon-search"></i>
              </button>
            </div>
          </div>
        </form>
        <br>
        <button type="button" name="button" class="btn btn-default" v-on:click="advanced=true;">Advanced Search>></button>
        <div class="form-inline" v-if="advanced">

          <div class="form-group">
            <label for="usr">Stars:</label>
            <input type="text" class="form-control" id="usr" v-model="nbrStars">
          </div>
          <div class="form-group">
            <label for="usr">Forks:</label>
            <input type="text" class="form-control" id="usr" v-model="nbrForks">
          </div>
          <div class="form-group">
            <label for="usr">activity:</label>
            <input type="text" class="form-control" id="usr" v-model="nbrActivity">
          </div>
        </div>
      </div>
    </div>
    <div class="row">
      <table class="table">
      <thead>
        <tr>
          <th><center>Repositorie name</center></th>
          <th><center>Stars</center></th>
          <th><center>Forks</center></th>
          <th><center>Last Update</center></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="repo in repoLists">
          <td>{{repo.name}}</td>
          <td>{{repo.name}}</td>
          <td>{{repo.name}}</td>
          <td>{{repo.name}}</td>
        </tr>
      </tbody>
    </table>
    <ul class="pager">
        <li><a class="btn" v-on:click="currPage--;searchKey();">Previous</a></li>
        <li><a class="btn" v-on:click="currPage++;searchKey();">Next</a></li>
    </ul>
    </div>
    </div>
  </div>
</template>


<script>

export default {

  name: 'GitSearch',
  data () {
    return {
      currPage: 0,
      nbrStars: 0,
      nbrForks: 0,
      nbrActivity: 0,
      msg: 'Welcome to Your Vue.js App',
      key: '',
      stars: false,
      forks: false,
      activity: false,
      advanced: false,
      nbrStars: undefined,
      repoLists: []
    }

  },
  methods: {

    searchKey: function() {
      var s="";
      if (this.stars) {
          s="&sort=stars";
      }
      if (this.forks) {
          s="&sort=forks";
      }
      if (this.activity) {
          s="&sort=activity";
      }

      var self= this;
      console.log(s);
      this.$http.get('https://api.github.com/search/repositories?q='+this.key+s+'&page='+this.currPage+'&per_page=100').then(response => {
            response.json().then(function (data) {
              if(self.nbrStars>0 ){
                self.repoLists= data.items.filter(function(e) {
                  return e.stargazers_count> self.nbrStars;
                });
              }
              else if(self.nbrForks>0){
                self.repoLists= data.items.filter(function(e) {
                  return e.forks_count> self.nbrStars;
                });
              }
              else if(self.nbrActivity>0){
                self.repoLists= data.items.filter(function(e) {
                  return e.updated_at> self.nbrStars;
                });
              }
              else{
                  self.repoLists= data.items;
              }

              console.log(data.total_count);
              console.log(self.repoLists);

            });
      }, response => {
        // error callback
      });
    }


  }

}

$(document).ready(function(e){
    $('.search-panel .dropdown-menu').find('a').click(function(e) {
		e.preventDefault();
		var param = $(this).attr("href").replace("#","");
		var concept = $(this).text();
		$('.search-panel span#search_concept').text(concept);
		$('.input-group #search_param').val(param);
	});
});

</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
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
