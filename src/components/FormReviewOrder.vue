<template>
  <div>
    <h1 class="title">Confirme sua Assinatura:</h1>

    <h2 class="subtitle">
      Estamos quase lá. Sua saúde está a um passo de ser bem cuidada por nossos especialistas.
    </h2>

    <div class="summary">
      <h3>Assinatura</h3>

      <p class="description">
        Vamos enviar um relatório completo da sua saúde a cada semana.
      </p>

      <div class="plans">
        <div class="plan active-plan" width=600px>
          <div class="weight" style="background-color:#FFFFFF;">          
            <img :src=wizardData.plan.url alt="Plano" width="250" height="250">
          </div>
          <div class="description">
            <span class="title">
              {{ wizardData.plan.name }}
            </span>
            <span class="description">
              {{ wizardData.plan.description }}
            </span>
          </div>

          <div class="price">
            <span class="dollar-sign">$</span>
            <span class="number">{{totalPrice}}</span>
          </div>
        </div>
      </div>

      <h3>
        Acrescente mais cuidados!
      </h3>

      <p class="description">
        Cuide de você mesmo, dando um upgrade no seu plano !
      </p>

      <div @change="submit" class="options">
        <div class="option">
          <input v-model="form.visita" type="checkbox" value="Visitas do especialista" id="visita">
          <label for="visita">1 visita mensal do especialista (+$20/mês)</label>
        </div>

        <div class="option">
          <input v-model="form.garantia" type="checkbox" value="Garantia extendida no equipamento" id="garantia" style="padding-left: 10px;">
          <label for="garantia">Não se preocupe com quedas ou riscos (+$20/mês)</label>
        </div>
      </div>

      <div class="address">
        <div class="w-2/3">
          <h3>Delivery</h3>
          <p class="description">
            Seu equipamento está pronto para ser embalado e enviado
          </p>
        </div>

        <div class="w-1/3">
          <h3>{{ wizardData.recipient }}</h3>
          <p class="leading-normal">
            {{ wizardData.address }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
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
          visita: false,
          garantia: false
        }
      }
    },
    computed: {
      totalPrice () {
        let total = this.wizardData.plan.price
        if(this.form.visita){
          total += 20
        }
        if(this.form.garantia){
          total += 20
        }
        return total
      }
    },
    validatio: {},
    methods: {
      submit() {
        this.$emit('update', {
          data: {
            visita: this.form.visita,
            garantia: this.form.garantia,
          },
          valid: true

        })
      }
    }
  }
</script>

<style scoped>

</style>