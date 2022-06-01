<template>
  <div id="app">
    <header>
      <h1>Table Vue Json</h1>
    </header>
    <main>
      <table class="table">
        <tr>
          <th>First Name</th>
          <th @click="sort('last_name')" style="cursor: pointer;">Last Name</th>
          <th>Email</th>
          <th>City</th>
          <th>Car</th>
        </tr>
        <tr v-for="user in sortedUsers" :key="user.id">
          <td>{{user.first_name}}</td>
          <td>{{user.last_name}}</td>
          <td>{{user.email}}</td>
          <td>{{user.city}}</td>
          <td>{{user.car}}</td>
        </tr>
      </table>
      <div class="d-grid button-group">
        <button class="btn btn-lg btn-primary" type="button" @click="prevPage">Previous</button>
        <button class="btn btn-lg btn-primary" type="button" @click="nextPage">Next</button>
      </div>
    </main>
  </div>
</template>

<script>
import usersData from "/Applications/MAMP/htdocs/tablejson/person_list.json";

export default {
  data() {
    return {
      users: usersData,
        currentSort:'first_name',
        currentSortDir:'asc',
        pageSize:8,
        currentPage:1
    };
  },
  methods:{
    sort:function(s) {
      if(s === this.currentSort) {
        this.currentSortDir = this.currentSortDir==='asc'?'desc':'asc';
      }
      this.currentSort = s;
    },
    nextPage:function() {
      if((this.currentPage*this.pageSize) < this.users.length) this.currentPage++;
    },
    prevPage:function() {
      if(this.currentPage > 1) this.currentPage--;
    }
  },
  computed:{
  //eslint-disable-next-line vue/no-side-effects-in-computed-properties
    sortedUsers:function() {
      return this.users.sort((a,b) => {
        let modifier = 1;
        if(this.currentSortDir === 'desc') modifier = -1;
        if(a[this.currentSort] < b[this.currentSort]) return -1 * modifier;
        if(a[this.currentSort] > b[this.currentSort]) return 1 * modifier;
        return 0;
      }).filter((row, index) => {
        let start = (this.currentPage-1)*this.pageSize;
        let end = this.currentPage*this.pageSize;
        if(index >= start && index < end) return true;
      });
    }
}
};

</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: "Lucida Sans", "Lucida Sans Regular", "Lucida Grande",
    "Lucida Sans Unicode", Geneva, Verdana, sans-serif;
}

header{
    box-shadow: 0 2px 8px rgb(0 0 0 / 26%);
    margin: 3rem auto;
    border-radius: 10px;
    padding: 1rem;
    background-color: #4fc08d;
    color: white;
    text-align: center;
    width: 90%;
    max-width: 40rem;
}

.button-group {
  margin: auto;
  width: 50%;
  padding: 10px;
  display: flex;
  justify-content: space-between;
}
</style>