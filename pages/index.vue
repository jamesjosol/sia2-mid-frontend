<template>
  <div>
    <NavBar />
    <div class="container">
      <h2>Welcome to my Web App</h2>
      <p v-if="$auth.loggedIn">
        User: {{ $auth.user.name }}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  auth: false,
}
</script>
