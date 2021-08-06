<template>
  <div class="section-detail">
    <Navbar :name="data.first_name" />
    <v-container style="padding-top: 72px">
      <v-card class="box-card">
        <v-img
          :src="data.avatar"
          height="380%"
        ></v-img>
        <v-card-title>{{ data.first_name }} {{ data.last_name }}</v-card-title>
        <v-card-subtitle>{{ data.email }}</v-card-subtitle>
      </v-card>
    </v-container>
  </div>
</template>

<script>
import CallApiLibrary from '@/lib/callApiLibrary'
export default {
  mixins: [ CallApiLibrary ],
  data () {
    return {
      title: 'Detail User',
      data: {}
    }
  },
  created () {
    this.getData()
  },
  methods: {
    getData () {
      this.callApiGet(`/api/users/${this.$route.params.id}`, responseData => {
        this.data = responseData.data
      }, responseData => {
      }, responseData => {
      }, true, true)
    }
  }
}
</script>
