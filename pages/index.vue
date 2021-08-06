<template>
  <div class="section-home">
    <v-container>
      <div class="box-header">
        <h1 class="title my-4">List User</h1>
        <v-btn to="/login" color="info">Login</v-btn>
      </div>
      <v-row>
        <v-col
          v-for="(data_list, index) in data"
          :key="index"
          cols="12"
          sm="12"
          md="4"
          lg="3"
          @click="toDetail(data[index].id)"
        >
          <v-card
            class="data-card"
          >
            <v-img
              :src="data[index].avatar"
              height="380px"
            ></v-img>
            <v-card-title>{{ data[index].first_name }} {{ data[index].last_name }}</v-card-title>
            <v-card-subtitle>{{ data[index].email }}</v-card-subtitle>
          </v-card>
        </v-col>
        <v-col
          cols="12"
          sm="12"
          md="4"
          lg="3"
        >
          <v-card
            @click="dialog = true"
            class="data-card">
            <div class="box-image">
              <img src="@/assets/icons/Icon-add-gray.svg" alt="icons">
            </div>
            <v-card-title class="justify-center pb-9">Add User</v-card-title>
          </v-card>
          <v-dialog
            v-model="dialog"
            width="800"
          >
            <template>
            </template>

            <v-card>
              <v-card-title class="text-h5 lighten-2">
                Create New user
              </v-card-title>

              <div class="box-data-modal px-5 py-3">
                <v-text-field
                  v-model="name"
                  label="Name"
                  outlined
                ></v-text-field>
                <v-text-field
                  v-model="job"
                  label="Job"
                  outlined
                ></v-text-field>
              </div>

              <v-divider></v-divider>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn
                  color="red lighten-2"
                  text
                  @click="dialog = false"
                >
                  Cancel
                </v-btn>
                <v-btn
                  color="info"
                  text
                  @click="addUser()"
                >
                  Create
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import CallApiLibrary from '@/lib/callApiLibrary'
export default {
  mixins: [ CallApiLibrary ],
  data () {
    return {
      title: 'List User',
      data: [],
      dialog: false,
      name: '',
      job: ''
    }
  },
  head () {
    return {
      title: this.title
    }
  },
  created () {
    this.getData()
  },
  methods: {
    getData () {
      this.callApiGet('api/users/', responseData => {
        this.data = responseData.data
      }, responseData => {
        this.showNotificationMessage('error')
      }, responseData => {
      }, true, true)
    },
    toDetail (id) {
      this.$router.push('/detail/' + id)
    },
    addUser () {
      const postData = {
        name: this.name,
        job: this.job
      }
      this.callApiPost('/api/users', postData, responseData => {
        this.showNotificationMessage('Success')
        this.dialog = false
        this.name = ''
        this.job = ''
        this.$router.push('/')
      }, responseData => {
      }, responseData => {
      }, true, true)
    }
  }
}
</script>
