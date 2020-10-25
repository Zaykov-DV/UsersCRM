<template>
    <div class="wrapper-content wrapper-content--fixed">
        <section>
            <div class="container">
                <h1>Users page</h1>
                <!--table-->
                <table>
                  <!--table head-->
                  <thead>
                    <tr>
                      <th @click="sort('first')">Name 	&darr;</th>
                      <th @click="sort('last')">Last Name 	&darr;</th>
                      <th @click="sort('age')">Age 	&darr;</th>
                      <th @click="sort('gender')">Gender 	&darr;</th>
                      <th @click="sort('country')">Country 	&darr;</th>
                    </tr>
                  </thead>
                  <!--table body-->
                  <tbody>
                    <tr v-for="user in usersSort" :key="user.id.value">
                      <td>
                        <img :src="user.picture.thumbnail" :alt="user.name.first">
                        <span>
                          {{ user.name.first }}
                        </span>

                      </td>
                      <td> {{ user.name.last }}</td>
                      <td> {{ user.dob.age }}</td>
                      <td> {{ user.gender }}</td>
                      <td> {{ user.location.country }}</td>
                    </tr>
                  </tbody>

                </table>
              <!--/ table-->
              <p style="text-align: center"> page: {{ page.current }} , length: {{ page.length }} </p>
              <p style="text-align: center">debug: sort: {{ currentSort }} , dir: {{ currentSortDir }} </p>
            </div>
        </section>

      <!--  pagination    -->
      <div class="section">
        <div class="container">
          <div class="button-list">
            <div class="btn btnPrimary" @click="prevPage"> &larr; </div>
            <div class="btn btnPrimary" @click="nextPage"> &rarr; </div>
          </div>
        </div>
      </div>

    </div>
</template>

<script>
import axios from 'axios'
export default {
  data () {
    return {
      users: [],
      currentSort: 'name',
      currentSortDir: 'asc',
      page: {
        current: 1,
        length: 5
      }
    }
  },
  created () {
    axios
        .get('https://randomuser.me/api/?results=20')
         .then(response => {
           // console.log(response.data)
           this.users = response.data.results
         })
          .catch(error => {
            console.log(error)
          })

    // this.users = [
    //   { id: 1, name: 'Jack', age: 22, gender: 'male' },
    //   { id: 2, name: 'Alex', age: 24, gender: 'male' }
    // ]
  },
  computed: {
    usersSort () {
      return this.users.sort((a,b) => {
        let mod = 1
        if (this.currentSortDir === 'desc') mod =-1

        // name sort
          if (a.name[this.currentSort] < b.name[this.currentSort]) return -1 * mod
          if (a.name[this.currentSort] > b.name[this.currentSort]) return 1 * mod

        // age sort
          if (a.dob[this.currentSort] < b.dob[this.currentSort]) return -1 * mod
          if (a.dob[this.currentSort] > b.dob[this.currentSort]) return 1 * mod

        // gender sort
          if (a[this.currentSort] < b[this.currentSort]) return -1 * mod
          if (a[this.currentSort] > b[this.currentSort]) return 1 * mod

        // gender sort
        if (a.location[this.currentSort] < b.location[this.currentSort]) return -1 * mod
        if (a.location[this.currentSort] > b.location[this.currentSort]) return 1 * mod

        return 0
      } ).filter((row, index) => {
        let start = (this.page.current-1)*this.page.length
        let end = this.page.current * this.page.length
        if (index >= start && index < end ) return true
      } )
    }
  },
  methods: {
    sort (e) {
      if (e === this.currentSort) {
        this.currentSortDir =  (this.currentSortDir === 'asc') ? 'desc' : 'asc'
      }
      this.currentSort = e
    },
    // pagination
    prevPage () {
      if (this.page.current >1 ) this.page.current-=1
    },
    nextPage () {
      if ((this.page.current * this.page.length) < this.users.length ) this.page.current+=1
    }
  }
}
</script>

<style lang="scss" scoped>
img {
  width: 60px;
  height: auto;
  border-radius: 50%;
  margin-right: 16px;
}
.button-list {
  width: 100%;
  text-align: center;
}
.btn {
  border-radius: 60px;
  margin: 0 20px;
}
</style>