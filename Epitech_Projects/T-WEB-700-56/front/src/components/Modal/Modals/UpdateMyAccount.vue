<template>
  <q-card>
    <q-card-section
      class="bg-primary text-white"
    >
      <span class="text-h6">{{$t('labels.update_my_info')}}</span>
    </q-card-section>
    <q-separator/>
    <q-card-section>
      <q-input
        v-model="email"
        :label="$t('labels.user_object.email')"
        @blur="checkMail"
      />
    </q-card-section>
  </q-card>
</template>

<script>
export default {
  name: 'UpdateMyAccount',
  data () {
    return {
      email: '',
      me: null
    }
  },
  mounted () {
    this.loadData()
  },
  methods: {
    checkMail () {

    },
    loadData () {
      this.$axios({
        method: 'get',
        url: 'http://localhost:3000/api/me',
        headers: {
          Authorization: 'Bearer ' + this.$q.localStorage.getItem('jwt')
        }
      }).then((response) => {
        this.me = response.data.user
        this.email = this.me.email
      }).catch((error) => {
        // Test if the token isn't valid
        if (error.response.data.message === 'Invalid token') {
          this.$root.$emit('token-invalid')
        }
      })
    }
  }
}
</script>

<style scoped>

</style>
