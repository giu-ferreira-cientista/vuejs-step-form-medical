<template>
  <div class="">
    <div v-if="wizardInProgress" v-show="asyncState !== 'pending'">
      <keep-alive>
        <component
            ref="currentStep"
            :is="currentStep"
            @update="processStep"
            @updateAsyncState="updateAsyncState"
            :wizard-data="form"
        ></component>
      </keep-alive>
      <div class="progress-bar">
        <div :style="`width: ${progress}%;`"></div>
      </div>

      <!-- Actions -->
      <div class="buttons">
        <button
            @click="goBack"
            v-if="currentStepNumber > 1"
            class="btn-outlined"
        >Voltar
        </button>
        <button
            @click="nextButtonAction"  
            :disabled="!canGoNext"
            class="btn"
        >{{ isLastStep ? 'Completar Assinatura' : 'Próximo'  }}</button>
      </div>

      <!-- <pre><code>{{form}}</code></pre> -->
    </div>
    <div v-else>
      <h1 class="title">Obrigado pela sua assinatura!</h1><br>
      <h2 class="subtitle">
        Seu dispositivo está a caminho. <br><br>
        Seus dados serão enviados para o seu médico em tempo real, para que ele acompanhe os seus indicadores.<br><br>
        Parabéns por cuidar da sua saúde!
      </h2>
      <br>
      <center><a href=".">Novo Cadastro</a></center>
    </div>
    <div class="loading-wrapper" v-if="asyncState === 'pending'">
      <div class="loader">
        <img src="/spinner.svg" alt="">
        <p>Validando os dados...</p>
      </div>
    </div>
  </div>

</template>

<script>
import {postFormToDB} from "../api";
import FormPlanPicker from './FormPlanPicker'
import FormUserDetails from './FormUserDetails'
import FormAddress from './FormAddress'
import FormReviewOrder from './FormReviewOrder'
export default {
  name: 'FormWizard',
  components: {
    FormPlanPicker,
    FormUserDetails,
    FormAddress,
    FormReviewOrder
  },
  data () {
    return {
      canGoNext: false,
      currentStepNumber: 1,
      asyncState: null,
      steps: [
        'FormPlanPicker',
        'FormUserDetails',
        'FormAddress',
        'FormReviewOrder'
      ],
      form: {
        plan: null,
        email: null,        
        name: null,
        password: null,
        address: null,
        recipient: null,
        idade: null,
        sexo: null,
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
  computed: {
    isLastStep() {
      return this.currentStepNumber === this.length
    },
    wizardInProgress() {
      return this.currentStepNumber <= this.length
    },
    currentStep(){
      return this.steps[this.currentStepNumber - 1]
    },
    length(){
      return this.steps.length
    },
    progress () {
      return this.currentStepNumber/this.length * 100
    }
  },
  methods: {
    updateAsyncState(state) {
      this.asyncState = state
    },
    nextButtonAction(){
      if(this.isLastStep) {
        this.submitOrder()
      } else {
        this.goNext()
      }
    },
    submitOrder(){
      this.asyncState = 'pending'
      postFormToDB(this.form)
      .then(() => {
        console.log('form submitted', this.form)
        this.asyncState = 'success'
        this.currentStepNumber++
      })
    },
    processStep(step){
      Object.assign(this.form,step.data)
      this.canGoNext = step.valid
    },
    goBack () {
      this.currentStepNumber--
      this.canGoNext = true
    },
    goNext () {
      this.currentStepNumber++
      // this.canGoNext = false
      this.$nextTick(() => {
        this.canGoNext = !this.$refs.currentStep.$v.$invalid
        // this.$refs.currentStep.submit()
      })

    }
  }
}
</script>
