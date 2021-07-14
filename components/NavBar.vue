<template>
    <div class="mb-3">
      <b-modal id="logoutModal" title="Logout?" size="sm" button-size="sm" ok-title="Logout" ok-variant="danger" headerClass="p-2 border-bottom-0" footerClass="p-2 mt-2 border-top-0" @ok="onLogout">
          Are you sure you want to logout? <br>
      </b-modal>
      <b-navbar variant="info" type="dark">
        <div class="container">
            <b-navbar-brand href="/">SIA2 Midterm</b-navbar-brand>
            <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

            <b-collapse id="nav-collapse" is-nav>
              <b-navbar-nav class="ml-auto">
              
                <template v-if="$auth.loggedIn">
                  <b-nav-item href="/dashboard">Dashboard</b-nav-item>
                  <b-nav-item href="/books">Books</b-nav-item>
                  <b-nav-item-dropdown right>
                    <template #button-content>
                      <i class="fa fa-user"></i>&nbsp; {{ $auth.user.name }}
                    </template>
                    <b-dropdown-item href="#" @click.prevent="logout"><i class="fas fa-sign-out-alt"></i> Logout</b-dropdown-item>
                </b-nav-item-dropdown>
                </template>

                <template v-else>
                  <b-nav-item href="/register">Register</b-nav-item>
                  <b-nav-item href="/login">Login</b-nav-item>
                </template>

                
              </b-navbar-nav>
            </b-collapse>
            
        
        </div>
      </b-navbar>
    </div>
</template>

<script>
export default {
  methods: {
    async onLogout() {
      await this.$auth.logout()
    },
    logout() {
      this.$bvModal.show('logoutModal')
    }
  }
}
</script>