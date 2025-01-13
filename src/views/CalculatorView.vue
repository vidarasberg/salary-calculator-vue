<script setup lang="ts">
import { computed, ref } from 'vue'
import NumberInput from '../components/NumberInput.vue'
import { CONSTANTS_SALARY } from '@/constants'
import RoundedValue from '@/components/RoundedValue.vue'

const hours = ref(160)
const hourlyRate = ref(1000)
const grossSalary = ref(55000)
const pension = ref(3000)
const salaryFirstThreeMonthsAfterFreelance = ref(55000)

const pensionTax = computed(() => pension.value * CONSTANTS_SALARY.PENSION_TAX)

const monthlyIncome = computed(
  () => hours.value * hourlyRate.value * CONSTANTS_SALARY.YOUR_CUT_OF_THE_POT,
)
const employerFee = computed(() => grossSalary.value * CONSTANTS_SALARY.EMPLOYER_FEE)
const totalMonthlyCost = computed(
  () => grossSalary.value + pension.value + pensionTax.value + employerFee.value,
)
const remaining = computed(() => monthlyIncome.value - totalMonthlyCost.value)

const costThreeMonths = computed(
  () =>
    salaryFirstThreeMonthsAfterFreelance.value * 3 +
    salaryFirstThreeMonthsAfterFreelance.value * CONSTANTS_SALARY.EMPLOYER_FEE * 3,
)

const numberOfMonthsToHaveEnoughForFirstThreeMonthsAfterFreelance = computed(
  () => costThreeMonths.value / remaining.value,
)
</script>

<template>
  <main>
    <h1>Lönekalkylator</h1>
    <h2>Intäkter / Debitering</h2>
    <NumberInput v-model="hours" label="Debiterade timmar" id="hours-input" />
    <NumberInput v-model="hourlyRate" label="Timarvode" id="hourly-rate-input" />
    <RoundedValue label="Månadsintäkt" :value="monthlyIncome" />

    <h2>Löneuttag / Andra utgifter</h2>
    <NumberInput v-model="grossSalary" label="Gross salary" id="gross-salary-input" />
    <RoundedValue label="Arbetsgivaravgift" :value="employerFee" />
    <NumberInput v-model="pension" label="Pension" id="pension-input" />
    <RoundedValue label="Skatt för pension" :value="pensionTax" />
    <RoundedValue label="Total kostnad (löneuttag + pension)" :value="totalMonthlyCost" />
    <RoundedValue label="Kvar i potten varje månad" :value="remaining" />

    <h2>Tiden som egen</h2>
    <NumberInput
      v-model="salaryFirstThreeMonthsAfterFreelance"
      label="Salary first three months after starting to freelance"
      id="salaryFirstThreeMonthsAfterFreelance-input"
    />
    <RoundedValue
      label="Months needed for salary"
      :value="numberOfMonthsToHaveEnoughForFirstThreeMonthsAfterFreelance"
    />
  </main>
</template>
