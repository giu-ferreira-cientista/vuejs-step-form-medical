<template>
  <div>
    <h1 class="title">Sua Conta</h1>

    <h2 class="subtitle">
      Crie sua conta para salvar seus dados
    </h2>

    <form v-if="!loggedIn" @input="submit" class="form">
      <div class="form-group">
        <label class="form-label" for="email">Email</label>
        <input @blur="checkIfUserExists" type="text" v-model="$v.form.email.$model" placeholder="seu@email.com" class="form-control" id="email">
        <div v-if="$v.form.email.$error && !$v.form.email.required" class="error">email é requerido</div>
        <div v-if="$v.form.email.$error && !$v.form.email.email" class="error">email é requerido</div>
      </div>
      <div class="form-group">
        <label class="form-label" for="password">Senha</label>
        <input v-model="$v.form.password.$model" type="password" placeholder="Password Super Secreto" class="form-control" id="password">
        <div v-if="$v.form.password.$error && !$v.form.password.required" class="error">password é requerido</div>
        <div v-if="$v.form.password.$error && !$v.form.password.correct" class="error">password é requerido - tente de novo</div>
      </div>

      <div v-if="existingUser" class="class-group">
        <button @click.prevent="login" class="btn">Login</button>
      </div>

      <div class="form-group">
        <label class="form-label" for="name">Nome</label>
        <input v-model="$v.form.name.$model" type="text" placeholder="Como devemos lhe chamar?" class="form-control" id="name">
        <div v-if="$v.form.name.$error" class="error">nome é requerido</div>
      </div>
    </form>
  </div>
</template>

<script>
import {authenticateUser} from "../api";
import {required, email} from 'vuelidate/lib/validators'
  export default {
    data () {
      return {
        form: {
          email: null,
          password: null,
          name: null,
        },
        emailCheckedInDB: false,
        existingUser: false,
        wrongPassword: false
      }
    },
    computed: {
      loggedIn() {
        return this.existingUser && this.form.name
      }
    },
    validations: {
      form: {
        email: {
          required,
          email
        },
        password: {
          required,
          correct(){
            return !this.wrongPassword
          }
        },
        name: {
          required
        }
      }
    },
    methods: {
      checkIfUserExists(){
          this.existingUser = false
          this.emailCheckedInDB = true
          this.$emit('updateAsyncState', 'success')
      },
      login(){
        this.wrongPassword = false
        if (!this.$v.form.email.$invalid) {
          this.$emit('updateAsyncState', 'pending')
          return authenticateUser(this.form.email, this.form.password)
              .then(user => {
                // LOGGED IN
                this.form.name = user.name
                this.submit()
                this.$emit('updateAsyncState', 'success')
              })
              .catch(() => {
                // WRONG PASSWORD?
                this.wrongPassword = true
                this.$emit('updateAsyncState', 'success')
              })
        }
      },
      reset() {
       this.form.email = null,
       this.form.password = null,
       this.form.name = null,
       this.emailCheckedInDB = false,
       this.wrongPassword = false,
       this.existingUser = false,
       this.$v.$reset()
      },
      submit() {
          this.$emit('update', {
            data: {
              email: this.form.email,
              password: this.form.password,
              name: this.form.name
            },
            valid: !this.$v.$invalid
          })
      }
    }
  }
</script>

<style scoped>

</style>
