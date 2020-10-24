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
                      <th @click="sort('first')">Name</th>
                      <th @click="sort('last')">Last Name</th>
                      <th @click="sort('age')">Age</th>
                      <th @click="sort('gender')">Gender</th>
                      <th @click="sort('country')">Country</th>
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
              <p>debug: sort: {{ currentSort }} , dir: {{ currentSortDir }} </p>
            </div>
        </section>
    </div>
</template>

<script>
import axios from 'axios'
export default {
  data () {
    return {
      users: [],
      currentSort: 'name',
      currentSortDir: 'asc'
    }
  },
  created () {
    axios
        .get('https://randomuser.me/api/?results=50')
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
      } )
    }
  },
  methods: {
    sort (e) {
      if (e === this.currentSort) {
        this.currentSortDir =  (this.currentSortDir === 'asc') ? 'desc' : 'asc'
      }
      this.currentSort = e
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
</style>