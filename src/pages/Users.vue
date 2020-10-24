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
                      <th>Name</th>
                      <th>Last Name</th>
                      <th>Age</th>
                      <th>Gender</th>
                      <th>Country</th>
                    </tr>
                  </thead>
                  <!--table body-->
                  <tbody>
                    <tr v-for="user in users" :key="user.id.value">
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
            </div>
        </section>
    </div>
</template>

<script>
import axios from 'axios'
export default {
  data () {
    return {
      users: []
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