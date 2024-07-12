<template><div class="row q-mt-lg q-mb-xs">
    <q-btn type="submit"  rounded no-caps class="q-px-lg" color="primary">
    <CalculatorIcon style="padding: 2px;"/>
    <span class="q-pa-xs dark">
      Calculate Repayments
    </span>
    </q-btn>
    </div></template>

    <script setup>
    import CalculatorIcon from '../components/CalculatorIcon.vue'
</script>

<style scoped>
.dark{
  color: hsl(202, 55%, 16%);
  font-family: 'Jakarta-Bold',sans-serif;
}
</style>