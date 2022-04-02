<template>
  <div>
    <h1 class="title">Detalhes do Paciente</h1>

    <h2 class="subtitle">
      Queremos saber mais de você! Para isso precisamos de algumas informações suas...
    </h2>

    <form class="form" @input="submit">
      <div class="form-group">
        <label class="form-label" for="delivery_name">Nome</label>
        <input v-model="$v.form.recipient.$model" type="text" placeholder="Nome do Paciente" class="form-control" id="delivery_name">
        <div v-if="$v.form.recipient.$error" class="error">campo requerido</div>
      </div>
      <div class="form-group">
        <label class="form-label" for="idade">Idade</label>
        <input v-model="form.idade" type="text" placeholder="Idade do Paciente" class="form-control" id="idade">        
      </div>
      <div @change="submit" style="margin-right: 197px;">
        <div class="form-group" >
          <label class="form-label" for="sexom">Sexo</label>
          <input type="radio" id="sexom" v-model="form.sexo" v-bind:value="1" style="justify-content: center;">&nbsp;masculino
          &nbsp;<input type="radio" id="sexof" v-model="form.sexo" v-bind:value="0" style="justify-content: center;">&nbsp;feminino
        </div>
      </div>
      <div class="form-group">
        <label class="form-label" for="delivery_name">Peso</label>
        <input v-model="form.peso" type="text" placeholder="Peso do Paciente" class="form-control" id="delivery_name">        
      </div>
      <div class="form-group">
        <label class="form-label" for="delivery_name">Altura</label>
        <input v-model="form.altura" type="text" placeholder="Altura do Paciente" class="form-control" id="delivery_name">        
      </div>
      <div class="form-group">
        <label class="form-label" for="address">Endereco</label>
        <textarea v-model="$v.form.address.$model" placeholder="Av. Joao Afonso, 49 apto 1200" rows="3" class="form-control" id="address"></textarea>
        <div v-if="$v.form.address.$error" class="error">campo requerido</div>
      </div>
      <div @change="submit" class="options">
        <div class="form-group" style="justify-content: left;padding-left: 120px;">
          <input v-model="form.fumante" type="checkbox" value="Fumante" id="fumante">
          <label for="fumante" style="padding-left: 10px;">Fuma ou já fumou?</label>      
        </div>
        <div class="form-group" style="justify-content: left;padding-left: 120px;">
          <input v-model="form.historico" type="checkbox" value="historico" id="historico">
          <label for="historico" style="padding-left: 10px;">Histórico Familiar de doenças?</label>      
        </div>
        <div class="form-group" style="justify-content: left;padding-left: 120px;">
          <input v-model="form.gestante" type="checkbox" value="gestante" id="gestante">
          <label for="gestante" style="padding-left: 10px;">Gestante?</label>      
        </div>
        <div class="form-group" style="justify-content: left;padding-left: 120px;">
          <input v-model="form.avc" type="checkbox" value="avc" id="avc">
          <label for="avc" style="padding-left: 10px;">AVC?</label>      
        </div>
        <div class="form-group" style="justify-content: left;padding-left: 120px;">
          <input v-model="form.frutas" type="checkbox" value="frutas" id="frutas">
          <label for="frutas" style="padding-left: 10px;">Frutas?</label>      
        </div>
        <div class="form-group" style="justify-content: left;padding-left: 120px;">
          <input v-model="form.vegetais" type="checkbox" value="vegetais" id="vegetais">
          <label for="vegetais" style="padding-left: 10px;">Vegetais?</label>      
        </div>
        <div class="form-group" style="justify-content: left;padding-left: 120px;">
          <input v-model="form.alcool" type="checkbox" value="alcool" id="alcool">
          <label for="alcool" style="padding-left: 10px;">Alcool?</label>      
        </div>
        <div class="form-group" style="justify-content: left;padding-left: 120px;">
          <input v-model="form.coracao" type="checkbox" value="coracao" id="coracao">
          <label for="coracao" style="padding-left: 10px;">Doença de Coração?</label>      
        </div>
        <div class="form-group" style="justify-content: left;padding-left: 120px;">
          <input v-model="form.colesterol" type="checkbox" value="colesterol" id="colesterol">
          <label for="colesterol" style="padding-left: 10px;">Colesterol Alto?</label>      
        </div>
      </div>
    </form>
  </div>
</template>

<script>
  import {required} from 'vuelidate/lib/validators'
  export default {
    props: {
      wizardData: {
        type: Object,
        required: true
      }
    },
    data () {
      return {
        form: {
          recipient: this.wizardData.name,
          address: null,
          idade : null,
          sexo : null,
          peso: null,
          altura: null,
          fumante: false,
          historico: false,
          gestante: false,
          avc: false,
          frutas: false,
          vegetais: false,
          alcool: false,
          coracao: false,
          colesterol: false,
          visita: false,
          garantia: false

        }
      }
    },
    activated() {
      this.form.recipient = this.wizardData.name
    },
    validations: {
      form: {
        address: {
          required
        },
        recipient: {
          required
        }
        ,
        idade: {
          required
        },
        peso: {
          required
        },
        altura: {
          required
        }
      }
    },
    methods: {
      submit() {
          this.$emit('update', {
            data: {
              address: this.form.address,
              recipient: this.form.recipient,
              idade : this.form.idade,
              sexo : this.form.sexo,
              peso: this.form.peso,
              altura: this.form.altura, 
              fumante: this.form.fumante,
              historico: this.form.historico,
              gestante: this.form.gestante,
              avc: this.form.avc,
              frutas: this.form.frutas,
              vegetais: this.form.vegetais,
              alcool: this.form.alcool,
              coracao: this.form.coracao,
              colesterol: this.form.colesterol,
              visita: this.form.visita,
              garantia: this.form.garantia
            },
            valid: !this.$v.$invalid
           })
      }
    }
  }
</script>

<style scoped>

</style>