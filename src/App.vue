<template>
  <div id="app" class="container mx-auto py-6">
    <header>
      <div>
        <span style="font-weight: 700;text-transform: uppercase;color: #5c6162;font-size: .875rem;letter-spacing: .05em;padding-left: 0.5rem;padding-right: 0.5rem;padding-bottom: 10rem;">
            Pandas Health App
        </span><br>
        <span id="logo" style="background-size: 150px;padding-bottom: 10rem;">+++++++++++++++++++</span>       
      </div>
    </header>

    <div class="content">
      <FormWizard/>
    </div>
  </div>
</template>

<script>
import FormWizard from './components/FormWizard.vue'

export default {
  name: 'app',
  components: {
    FormWizard
  }
}
</script>