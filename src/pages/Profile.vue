<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <q-page>
    <q-card class="q-pa-md q-ma-md">
      <q-breadcrumbs>
        <q-breadcrumbs-el label="Home" icon="home" />
        <q-breadcrumbs-el class="text-grey-7" label="Profile" icon="health_and_safety" />
      </q-breadcrumbs>
    </q-card>
      <div class="q-ma-md q-mt-lg" style="height: fit-content;">
        <q-card>
          <div>
            <q-img src="bg_profile.jpg" style="height: 200px;" />

            <q-card-section>
              <div class="row col-12">
                <q-avatar style="background: blue; width: 200px; height: 200px; margin-bottom: -120px; transform: translateY(-70%);">
                  <img src="avatar.png" />
                </q-avatar>
                <div class="col text-h6 q-ml-lg text-capitalize text-blue-7 text-weight-bold">
                  {{dataUser.user.username}}
                  <q-btn @click="profile=true" dense flat text-color="blue-7" icon="edit" class="q-mr-md" size="10px">
                    <q-tooltip>
                      Edit
                    </q-tooltip>
                  </q-btn>
                  <div class="col text-caption text-grey">
                    Administrator system BLITS Ambulans.
                  </div>
                  <div class="col text-caption text-grey">
                    <q-icon name="email" /> {{dataUser.user.email}} | <q-icon name="location_on" /> {{dataUser.user.alamat}}
                  </div>
                </div>
              </div>
            </q-card-section>

            <q-card-section class="q-pt-none">
            </q-card-section>
          </div>
        </q-card>

        <q-dialog v-model="profile">
          <q-card class="my-card" flat bordered style="width: 600px; max-width: 60vw;">
            <q-item>
              <q-item-section avatar>
                <q-avatar>
                  <q-icon name="health_and_safety" size="40px" color="blue-7" />
                </q-avatar>
              </q-item-section>

              <q-item-section>
                <q-item-label>Data Profile</q-item-label>
                <q-item-label caption>
                  Ubah data profile pengguna
                </q-item-label>
              </q-item-section>

              <q-item-section class="col-1">
                <q-btn flat dense icon="close" class="float-right" color="grey-8" v-close-popup></q-btn>
              </q-item-section>
            </q-item>

            <q-separator />

            <q-form
              @submit="Profile (guid)">
              <q-card-section horizontal>
                <q-card-section class="q-gutter-md fit">
                  <q-input dense outlined v-model="username" label="Username"/>
                  <q-input dense outlined v-model="no_telpon" label="No. Telepon"/>
                </q-card-section>
                <q-separator vertical />
                <q-card-section class="q-gutter-md fit">
                  <q-input dense outlined v-model="email" label="Email"/>
                  <q-input dense outlined v-model="alamat" label="Alamat"/>
                </q-card-section>
              </q-card-section>
              <q-separator />
              <q-card-actions>
                <q-btn type="submit" flat color="primary">
                  Simpan
                </q-btn>
              </q-card-actions>
            </q-form>
          </q-card>
        </q-dialog>
      </div>
  </q-page>
</template>

<script>
import createToken from 'src/boot/create_token'
export default {
  data () {
    return {
      dataUser: this.$q.localStorage.getItem('dataUser'),
      username: null,
      email: null,
      no_telpon: null,
      alamat: null,
      profile: false
    }
  },
  methods: {
    Profile (guid) {
      this.$axios.put(`http://localhost:5050/users/user-update/${this.dataUser.user.guid}`, {
      // this.$axios.put(`http://192.168.16.8:5050/users/user-update/${this.dataUser.user.guid}`, {
        username: this.username,
        email: this.email,
        no_telpon: this.no_telpon,
        alamat: this.alamat
      }, createToken())
        .then((res) => {
          console.log(res)
          if (res.data.status) {
            this.$q.dialog({
              title: 'Peringatan',
              message: 'apakah anda yakin? klik ok untuk melanjutkan'
            }).onOk(() => {
              this.$router.push({ name: 'login' })
            })
          }
        })
    },
    getUser () {
      this.$axios.get('http://localhost:5050/users/get-role-admin', {
        username: this.username,
        email: this.email,
        no_telpon: this.no_telpon,
        alamat: this.alamat
      }, createToken())
        .then((res) => {
          console.log(res)
          this.data = res.data.data
          this.username = this.data[0].username
          this.email = this.data[0].email
          this.no_telpon = this.data[0].no_telpon
          this.alamat = this.data[0].alamat
        })
    }
  },
  mounted () {
    this.getUser()
  }
}
</script>

<style scoped>

</style>
