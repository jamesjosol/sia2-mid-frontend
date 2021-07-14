<template>
  <div>
    <b-form  @submit.stop.prevent>
      <label for="feedback-user">User ID</label>
      <b-form-input v-model="userId" :state="validation" id="feedback-user"></b-form-input>
      <b-form-invalid-feedback :state="validation">
        Your user ID must be 5-12 characters long.
      </b-form-invalid-feedback>
      <b-form-valid-feedback :state="validation">
        Looks Good.
      </b-form-valid-feedback>
     </b-form>
     <div>
  <b-button variant="primary" disabled>
    <b-spinner small></b-spinner>
    <span class="sr-only">Loading...</span>
  </b-button>

  <b-button variant="primary" disabled>
    <b-spinner small type="grow"></b-spinner>
    Loading...
  </b-button>
  <div class="search">
    <span class="fa fa-search"></span>
    <input placeholder="Search term">
  </div>
</div>
  </div>
</template>

<script>
  export default {
    auth: false,
    data() {
      return {
        userId: ''
      }
    },
    computed: {
      validation() {
        return this.userId.length > 4 && this.userId.length < 13
      }
    }
  }
</script>
<style scoped>
  body { margin: 30px; }

.search {
  position: relative;
  color: #aaa;
  font-size: 16px;
 }

.search input {
  width: 250px;
  height: 32px;
  background: #fcfcfc;
  border: 1px solid #aaa;
  border-radius: 5px;
  box-shadow: 0 0 3px #ccc, 0 10px 15px #ebebeb inset;
}

.search input { text-indent: 32px;}
.search .fa-search { 
  position: absolute;
  top: 10px;
  left: 10px;
}
</style>