<template>
  <q-layout class="bg-accent flex flex-center">
   <q-form @submit.prevent="calculateMortgage" class="flex bg-white" style="border-radius: 20px;">
    <div  style="max-width: 420px;" class="q-pa-lg">
   <div class="row justify-between q-mb-md">
    <div class="text-h6">
      <p style="font-family: 'Jakarta-Bold',sans-serif;" class="dark">Mortgage Calculator</p>
  </div>
 <div>
  <ClearAllButton @click="clearAll" />
</div>
   </div>
<div class="row q-mt-md q-mb-xs">
  <div class="col-12">
  <label for="amount" >Mortgage Amount</label>
  <q-input :rules="[validateInput]" v-model="amount" class="dark" id="amount" outlined dense  >
<template v-slot:prepend >
  <q-icon name="mdi-currency-gbp" size="xs" color="dark">
  </q-icon>
</template>
  </q-input>
</div>
</div>
<div class="row justify-between q-gutter-md"><div class="col">
  <label for="mortgageTerm">Mortgage Term</label>
<q-input :rules="[validateInput]" v-model="term" id="mortgageTerm" class="dark" outlined dense >
  <template v-slot:append>
    <span class="text-subtitle2">
years
</span>
  </template>
</q-input>
</div>
<div class="col">
<label for="interestRate">Interest Rate</label>
<q-input :rules="[validateInput]" v-model="rate" id="interestRate" class="dark" outlined dense >
  <template v-slot:append>
    <span class="text-subtitle1" style="padding: 0">
%
</span>
  </template>
</q-input>
</div>
</div>
<div class="q-mt-xs">
  <p class="q-my-sm">
    Mortgage Type
  </p>
  <div class="column" style="gap: 5px;">
  <div class="row radio-button" :class="selectedOption === 'opt1' ? 'selected' : ''">
  <q-radio   v-model="selectedOption" val="opt1" class="dark">Repayment</q-radio>
</div>
<div class="row radio-button " :class="selectedOption === 'opt2' ? 'selected' : ''">
  <q-radio v-model="selectedOption" val="opt2" class="dark" >Interest Only</q-radio>
</div>
<div v-if="errorMessage" class="error-message">{{ errorMessage }}</div>
</div>
</div>
<SubmitButton/>
</div>
<div v-if="!result" class="bg-dark column flex-center" style="max-width: 420px; border-radius: 0 20px 20px 60px">
  <EmptyResults/>
</div>
<div v-else class="bg-dark q-pa-lg" style="max-width: 420px; border-radius: 0 20px 20px 60px">
<p class="text-h6 text-white">Your results</p>
<p class="text-positive">Your results are shown below based on the information you provided.To adjust the results, edit the form and click "Calculate Repayments" again.</p>

<q-card class="bg-info" style="border-top:3.5px solid hsl(61, 70%, 52%) ">
  <q-card-section>
    <p class="text-positive">Your monthly repayments</p>
    <p class="text-h3 text-primary dark">£{{ result.monthlyRepayment }}</p>
  </q-card-section>
  <q-separator color="positive" inset />
  <q-card-section class="q-mt-sm">
    <span class="text-positive">Total you'll repay over the term</span>
    <p class="text-white text-h5 q-mt-xs">£{{ result.totalRepayment }}</p>
  </q-card-section>
</q-card>
 
</div>

   </q-form>
  </q-layout>
</template>

<script setup>
import EmptyResults from 'components/EmptyResults.vue'
import ClearAllButton from 'components/ClearAllButton.vue'
import SubmitButton from 'components/SubmitButton.vue'
import { ref} from 'vue'
 
const amount = ref('');
const term = ref('');
const rate = ref('');
const selectedOption = ref(null);
const errorMessage = ref('');
const result = ref(null);


const validateRadioButtons = () => {
      if (selectedOption.value === null) {
        errorMessage.value = 'This field is required';
        return false;
      }
      errorMessage.value = '';
      return true;
    };

const validateInput = (value) => {
      if (value === '' || value === null || value === undefined) {
        return 'This field is required';
      }
      if (!/^\d+(\.\d+)?$/.test(value)) {
        return 'Only numbers are allowed';
      }
      return true;
    };


    const formatNumber = (number) => {
  return new Intl.NumberFormat('en-US', {
    minimumFractionDigits: 2,
    maximumFractionDigits: 2
  }).format(number);
};

    const calculateMortgage = () => {
  const principal = amount.value;
  const annualInterestRate = rate.value / 100;
  const monthlyInterestRate = annualInterestRate / 12;
  const numberOfPayments = term.value * 12;


  let monthlyRepayment;
  let totalRepayment;

  if (selectedOption.value === 'opt1') {
    // Repayment mortgage formula
    monthlyRepayment = principal * (monthlyInterestRate * Math.pow(1 + monthlyInterestRate, numberOfPayments)) / (Math.pow(1 + monthlyInterestRate, numberOfPayments) - 1);
    totalRepayment = monthlyRepayment * numberOfPayments;
  } else if (selectedOption.value === 'opt2') {
    // Interest-only mortgage formula
    monthlyRepayment = principal * monthlyInterestRate;
    totalRepayment = (monthlyRepayment * numberOfPayments) + principal;
  } else {
    validateRadioButtons();
    console.error('Invalid mortgage type');
    return;
  }

  result.value = {
    monthlyRepayment: formatNumber(monthlyRepayment),
    totalRepayment: formatNumber(totalRepayment)
  };


};
const clearAll = () => {
  amount.value = '';
 term.value = '';
 rate.value = '';
selectedOption.value = null;
 errorMessage.value = '';
 result.value = null;
}

</script>

<style lang="scss" scoped>
*{
  font-family: 'Jakarta-Medium',sans-serif;
  color: $secondary;
}
.radio-button{
  border: 1px solid $positive;
  border-radius: 5px
}
.dark{
  color: $dark;
  font-family: 'Jakarta-Bold',sans-serif;
}

.selected{
  background-color: hsl(61, 70%, 92%);
}

label{
  margin-top: 5px;
}

.error-message {
  color: red;
  margin-top: 5px;
}
</style>
