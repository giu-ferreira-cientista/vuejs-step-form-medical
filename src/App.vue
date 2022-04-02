<template>
  <div id="app" class="container mx-auto py-6">
    <header>
      <div>
        <span style="font-weight: 800;text-transform: uppercase;color: #5c6162;font-size: .895rem;letter-spacing: .05em;padding-left: 0.5rem;padding-right: 0.5rem;padding-bottom: 10rem;">
            Clínica Pandas Tech Health Care
        </span><br>        
        <span id="logo" style="background-size: 160px;padding-bottom: 10rem;">------------------------------------------------------</span>       
        <br><span style="font-weight: 590;text-transform: uppercase;color: #5c6162;font-size: .695rem;letter-spacing: .05em;padding-left: 0.5rem;padding-right: 0.5rem;padding-bottom: 10rem;">powered by:</span>
                                                                               
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
