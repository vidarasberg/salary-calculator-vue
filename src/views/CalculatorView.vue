<script setup lang="ts">
import { computed, ref } from 'vue'
import NumberInput from '../components/NumberInput.vue'
import { CONSTANTS_SALARY } from '@/constants'
import RoundedValue from '@/components/RoundedValue.vue'

const initialState = {
  hours: 160,
  hourlyRate: 1000,
  grossSalary: 55000,
  pension: 3000,
  salaryFirstThreeMonthsAfterFreelance: 55000,
}

const state = ref(initialState)

const pensionTax = computed(() => state.value.pension * CONSTANTS_SALARY.PENSION_TAX)

const monthlyIncome = computed(
  () => state.value.hours * state.value.hourlyRate * CONSTANTS_SALARY.YOUR_CUT_OF_THE_POT,
)
const employerFee = computed(() => state.value.grossSalary * CONSTANTS_SALARY.EMPLOYER_FEE)
const totalMonthlyCost = computed(
  () => state.value.grossSalary + state.value.pension + pensionTax.value + employerFee.value,
)
const remaining = computed(() => monthlyIncome.value - totalMonthlyCost.value)

const costThreeMonths = computed(
  () =>
    state.value.salaryFirstThreeMonthsAfterFreelance * 3 +
    state.value.salaryFirstThreeMonthsAfterFreelance * CONSTANTS_SALARY.EMPLOYER_FEE * 3,
)

const numberOfMonthsToHaveEnoughForFirstThreeMonthsAfterFreelance = computed(
  () => costThreeMonths.value / remaining.value,
)

function handleReset() {
  state.value = { ...initialState }
}
</script>

<template>
  <main>
    <h1>Lönekalkylator</h1>
    <h2>Intäkter / Debitering</h2>
    <NumberInput v-model="state.hours" label="Debiterade timmar" id="hours-input" />
    <NumberInput v-model="state.hourlyRate" label="Timarvode" id="hourly-rate-input" />
    <RoundedValue label="Månadsintäkt" :value="monthlyIncome" />

    <h2>Löneuttag / Andra utgifter</h2>
    <NumberInput v-model="state.grossSalary" label="Gross salary" id="gross-salary-input" />
    <RoundedValue label="Arbetsgivaravgift" :value="employerFee" />
    <NumberInput v-model="state.pension" label="Pension" id="pension-input" />
    <RoundedValue label="Skatt för pension" :value="pensionTax" />
    <RoundedValue label="Total kostnad (löneuttag + pension)" :value="totalMonthlyCost" />
    <RoundedValue label="Kvar i potten varje månad" :value="remaining" />

    <h2>Tiden som egen</h2>
    <NumberInput
      v-model="state.salaryFirstThreeMonthsAfterFreelance"
      label="Salary first three months after starting to freelance"
      id="salaryFirstThreeMonthsAfterFreelance-input"
    />
    <RoundedValue
      label="Months needed for salary"
      :value="numberOfMonthsToHaveEnoughForFirstThreeMonthsAfterFreelance"
    />
    <button @click="handleReset">Reset</button>
  </main>
</template>
