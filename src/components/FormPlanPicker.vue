<template>
  <div>
    <h1 class="title">Escolha o seu Plano</h1>

    <h2 class="subtitle">
      Nós oferecemos os melhores dispositivos monitoradores de saúde do mercado!
    </h2>

    <div class="plans">
      <div
        v-for="plan in plans"
        :key="plan.price"
        @click="pickPlan(plan)"
        :class="{'active-plan': selectedPlan === plan}"
        class="plan"
      >
        <div class="weight" :style=plan.margin>          
          <img :src=plan.url alt="Plano" width="250" height="250">
        </div>
        <div class="description">
          <span class="title">
              {{plan.name}}
          </span>
          <span class="description">
              {{plan.description}}
          </span>
        </div>
        <div class="price">
          <span class="dollar-sign">$</span>
          <span class="number">{{plan.price}}</span>
        </div>
      </div>
    </div>
    <div v-if="$v.selectedPlan.$error" class="error">Você precisa escolher um plano pra continuar</div>
  </div>
</template>

<script>
  import {required} from 'vuelidate/lib/validators'

  export default {
    data() {
      return {
        plans: [
          {
            price: 35,
            url: 'https://img.ibxk.com.br/2022/03/07/07143225682287.jpg',
            name: 'Pulseira Pandas Band',
            description: 'A Pandas Band tem um kit de sensores muito interessante para monitorar seus treinos e acompanhar o básico da sua saúde.',
            margin: 'margin-top: 11px;'
          },
          {
            price: 45,
            url: 'https://m.media-amazon.com/images/I/61DwXW7cqZL._AC_SX522_.jpg',
            name: 'Sensor Peitoral Pandas Chest',
            description: 'Graças à cinta peitoral, o sensor de frequência cardíaca é colocado próximo ao coração, a forma ideal para obter leituras precisas da sua frequência cardíaca.',
            margin: 'margin-top: 11px;'
          },
          {
            price: 55,
            url: 'https://www.zema.com/ccstore/v1/images/?source=/file/v4179927317032866053/products/1416657_01.jpg',
            name: 'Pandas Smart Watch Plus',
            description: 'Para te ajudar a cuidar da sua saúde, o modelo conta com monitor de sono, aplicativo para medir o oxigênio no sangue e batimentos cardíacos com notificações de desempenho, além das demais funções tradicionais.',
            margin: 'margin-top: 17px;'
          }
        ],
        selectedPlan: null
      }
    },
    validations: {
      selectedPlan: {
        required
      }
    },
    methods: {
      pickPlan (plan) {
        this.selectedPlan = plan

        this.submit()
      },
      submit() {
        this.$emit('update', {
          data: {
            plan: this.selectedPlan
          },
          valid: !this.$v.$invalid
        })
      }

    }
  }
</script>