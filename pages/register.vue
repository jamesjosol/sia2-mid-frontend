<template>
    <div>
        <NavBar />
        <NoticeMessageModal :message="noticeMsg" :title="noticeTitle" />
        <div class="container">
            <div class="row">
                <div class="col-md-4 offset-md-4 mt-3 pb-4">
                    <b-card header="User Registration">
                        <form action="#" @submit.prevent="submitForm">
                            <div class="form-group register-form">

                                <label for="name">Full Name</label>
                                <span class="fas fa-user"></span>
                                <input type="text" class="form-control" v-model.trim="$v.name.$model" :class="{
                                    'is-invalid':$v.name.$error, 'is-valid':!$v.name.$invalid }">
                                
                                <div class="invalid-feedback">
                                    <span v-if="!$v.name.required">Name is required.</span>
                                    <span v-if="!$v.name.minLength">Name must have atleast {{ 
                                        $v.name.$params.minLength.min}} letters.</span>
                                    <span v-if="!$v.name.maxLength">Name must have at most {{ 
                                        $v.name.$params.maxLength.max}} letters.</span>
                                </div>
                            </div>
                            <div class="form-group register-form">

                                <label for="email">Email</label>
                                <span class="fas fa-at"></span>
                                <input type="email" class="form-control" v-model.trim="$v.email.$model" :class="{
                                    'is-invalid':$v.email.$error, 'is-valid':!$v.email.$invalid, ' is-invalid': isTaken}" @focus="resetTaken">
                              
                                <div class="invalid-feedback">
                                    <span v-if="!$v.email.required">Email is required.</span>
                                    <span v-if="!$v.email.email">This email is invalid.</span>
                                </div>
                            </div>
                            <div class="form-group register-form">

                                <label for="password">Password</label>
                                <span class="fas fa-lock"></span>
                                <input type="password" id="password" class="form-control" v-model.trim="$v.password.$model" :class="{
                                    'is-invalid':$v.password.$error, 'is-valid':!$v.password.$invalid }">
                               
                                <div class="invalid-feedback">
                                    <span v-if="!$v.password.required">Password is required.</span>
                                    <span v-if="!$v.password.minLength"> {{ $v.password.$params.minLength.min }} 
                                        characters minimum.</span>
                                </div>
                            </div>
                            <div class="form-group form-check">
                                <input type="checkbox" name="" id="showPassword" class="form-check-input mt-2" 
                                @click="toogleShowPassword" v-model="showPassword">
                                <label for="showPassword" class="form-check-label" style="font-size: 0.8em;">Show Password</label>
                            </div>
                            <div class="form-group register-form">

                                <label for="password">Confirm Password</label>
                                <span class="fas fa-lock"></span>
                                <input type="password" class="form-control" v-model.trim="$v.confirmPassword.$model" :class="{
                                    'is-invalid':$v.confirmPassword.$error, 'is-valid': (password != '') ? !$v.confirmPassword.$invalid : ''}">
                                
                                <div class="invalid-feedback">
                                    <span v-if="!$v.confirmPassword.sameAsPassword">Your password must be identical.</span>
                                </div>
                            </div>
                            <div class="form-group">
                                <template v-if="isBusy">
                                    <button class="btn btn-primary" type="submit" disabled><b-spinner small></b-spinner> Register</button>
                                </template>
                                <template v-else>
                                    <button class="btn btn-primary" type="submit">Register</button>
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
import { required, minLength, maxLength, email, sameAs, numeric, url } from 'vuelidate/lib/validators'

export default {
    auth: false,
    data() {
        return {
            name: '',
            email: '',
            password: '',
            confirmPassword: '',
            showPassword: false,
            user: {},
            isBusy: false,
            isTaken: false,
            noticeMsg: '',
            noticeTitle: ''
        }
    },
    validations: {
        name: {
            required,
            minLength: minLength(6),
            maxLength: maxLength(18)
        },
        email: {
            required,
            email,
        },
        password: {
            required,
            minLength: minLength(8)
        },
        confirmPassword: {
            sameAsPassword: sameAs('password')
        }
    },
    methods: {
        toogleShowPassword() {
            var show = document.getElementById('password')
            if(this.showPassword == false) {
                this.showPassword = true
                show.type = "text"
            }else {
                this.showPassword = false
                show.type = "password"
            }
        },
        submitForm() {
            this.$v.$touch()
            if(!this.$v.$invalid) {
                this.onSubmit()
            }
        },
        async onSubmit() {
            this.isBusy = true
            this.user = {
                name: this.name,
                email: this.email,
                password: this.password
            }
            this.$axios.post('/api/register', this.user)
            .then((res) => {
                if(res.status==202) {
                    this.$bvModal.show('NoticeMsgModal')
                    this.noticeMsg = res.data.message
                    this.noticeTitle = "Notification"
                    setTimeout(() => {
                        this.$bvModal.hide('NoticeMsgModal')
                    }, 2 * 1000)
                    this.name = ''
                    this.email = ''
                    this.password = ''
                    this.confirmPassword = ''
                    this.isBusy = false
                }
            })
            .catch((err) => {
                
                if(err.response.status==400) {
                    
                    this.isTaken = true
                    err.response.data.error.forEach(element => {
                        this.noticeMsg = element + '\n'
                    });
                    // this.noticeMsg = err.response.data.error
                    this.noticeTitle = "Notification"
                    console.log(err.response)
                    
                }else {
                    
                    this.noticeMsg = "Oppss... Something went wrong."
                    this.noticeTitle = "Registration Failed!"
                }
                
                this.$bvModal.show('NoticeMsgModal')
                setTimeout(() => {
                    this.$bvModal.hide('NoticeMsgModal')
                }, 2 * 1000) 
                this.isBusy = false
            })
        },
        resetTaken() {
            if(this.isTaken) {
                this.isTaken = false
            }
        }
    }
}
</script>

<style scoped>
.register-form {
    position: relative;
}

.register-form input { text-indent: 25px;}

.register-form .fa-at{ 
  position: absolute;
  top: 43px;
  left: 10px;
}

.register-form .fa-lock{
    position: absolute;
    top: 43px;
    left: 11px;
}

.register-form .fa-user{
    position: absolute;
    top: 43px;
    left: 11px;
}
</style>