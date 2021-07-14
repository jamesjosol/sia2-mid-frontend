<template>
    <div>
        <NavBar />
        <NoticeMessageModal :message="noticeMsg" :title="noticeTitle" />
        <div class="container">
            <div class="row">
                <div class="col-md-4 offset-md-4 mt-3">
                    <b-card header="User Login">
                        <form action="#" @submit.prevent="login">
                            <div class="form-group login-form">

                                <label for="email">Email</label>
                                <span class="fas fa-at"></span>
                                <input type="email" class="form-control" v-model.trim="$v.email.$model" :class="{
                                    'is-invalid':$v.email.$error, ' is-invalid': invalidCreds }" @focus="resetInvalidCreds">
                                <div class="invalid-feedback">

                                    <span v-if="!$v.email.required">Email is required.</span>
                                    <span v-if="!$v.email.email">This email is invalid.</span>
                                </div>
                                
                            </div>
                            <div class="form-group login-form">

                                <label for="password">Password</label>
                                <span class="fas fa-lock"></span>
                                <input type="password" id="password" class="form-control" v-model.trim="$v.password.$model" :class="{
                                    'is-invalid':$v.password.$error, ' is-invalid': invalidCreds }" @focus="resetInvalidCreds">
                               
                                <div class="invalid-feedback">
                                    <span v-if="!$v.password.required">Password is required.</span>
                                    <span v-if="!$v.password.minLength">Password is too short.</span>
                                </div>
                            </div>
                            <div class="form-group">
                                <template v-if="loggingIn">
                                    <button class="btn btn-primary" disabled><b-spinner class="" small></b-spinner>  Login</button>
                                </template>
                                <template v-else>
                                    <button class="btn btn-primary" type="submit"><i class="fas fa-sign-in-alt"></i> Login</button>
                                </template>
                            </div>
                        </form>
                    </b-card>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { required, minLength, email} from 'vuelidate/lib/validators'
export default {
    data() {
        return {
            email: '',
            password: '',
            creds: {},
            loggingIn: false,
            noticeMsg: '',
            noticeTitle: '',
            invalidCreds: false,
        }
    },
    validations: {
         email: {
            required,
            email,
        },
        password: {
            required,
            minLength: minLength(8),
        },
    },
    methods: {
        login() {
            this.$v.$touch()
            if(!this.$v.$invalid) {
                this.onLogin()
            }
        },
        async onLogin() {
            this.loggingIn = true
            this.creds = {
                email: this.email,
                password: this.password
            }
            await this.$auth.loginWith('local', {data:this.creds})
            .then(() => {

            })
            .catch((err) => {
                // alert(err.message)
                // console.log(err)
                if(err.response.status==401) {
                
                    this.noticeMsg = "Invalid Username & Password."
                    this.noticeTitle = "Unauthorized"
                    this.invalidCreds = true
                }else {
                    this.noticeMsg = "Oppss... Something went wrong."
                    this.noticeTitle = "Login Failed!"
                }

                this.$bvModal.show('NoticeMsgModal')
                setTimeout(() => {
                    this.$bvModal.hide('NoticeMsgModal')
                }, 2 * 1000) 
                this.loggingIn = false
            })
        },
        resetInvalidCreds() {
            if(this.invalidCreds) {
                this.invalidCreds = false
            }
        }
    }
}
</script>

<style scoped>
.login-form {
    position: relative;
}

.login-form input { text-indent: 25px;}

.login-form .fa-at{ 
  position: absolute;
  top: 43px;
  left: 10px;
}

.login-form .fa-lock{
    position: absolute;
    top: 43px;
    left: 11px;
}
</style>