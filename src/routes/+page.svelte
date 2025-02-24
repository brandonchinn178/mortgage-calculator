<script lang="ts">
  import Slider from "$lib/Slider.svelte"

  const MAX_TOTAL_HOUSE = 5000000
  const MAX_DOWN_PAYMENT_PERCENT = 100
  const MAX_INTEREST_RATE_PERCENT = 10
  const MAX_LOAN_TERM = 30

  // TODO:
  //   * tax
  //   * insurance
  //   * HOA
  let data = $state({
    totalHouse: 500000,
    downPaymentPercent: 20,
    interestRatePercent: 6.5,
  })

  // TODO: move to state
  const loanTermYears = 30

  //  M = P * (J/(1-(1+J) ** -N))

  // https://www.hughcalc.org/formula.php
  // TODO: allow making input, with down payment as output
  const loanAmount = $derived(data.totalHouse * (1 - data.downPaymentPercent / 100))
  const monthlyInterest = $derived((data.interestRatePercent / 100) / 12)
  const monthlyPayment = $derived(loanAmount * monthlyInterest / (1 - (1 + monthlyInterest) ** -(loanTermYears * 12)))

  // TODO: allow user to specify downpayment in either dollar or percent

  // TODO: show amortization table
</script>

<header>
  <h1>Interactive Mortgage Calculator</h1>
</header>
<main>
  <div class="calculator-grid">
    <label for="total-house">Total house cost</label>
    <div class="field dollar">
      <input
        id="total-house"
        type="number"
        max={MAX_TOTAL_HOUSE}
        step="10000"
        bind:value={data.totalHouse}
      />
    </div>
    <Slider
      max={MAX_TOTAL_HOUSE}
      step={10000}
      tickInterval={250000}
      bind:value={data.totalHouse}
    />

    <label for="down-payment">Down payment</label>
    <div class="field percentage">
      <input
        id="down-payment"
        type="number"
        max={MAX_DOWN_PAYMENT_PERCENT}
        bind:value={data.downPaymentPercent}
      />
    </div>
    <Slider
      max={MAX_DOWN_PAYMENT_PERCENT}
      tickInterval={10}
      bind:value={data.downPaymentPercent}
    />

    <label for="interest-rate">Interest rate</label>
    <div class="field percentage">
      <input
        id="interest-rate"
        type="number"
        step="0.1"
        max={MAX_INTEREST_RATE_PERCENT}
        bind:value={
          () => data.interestRatePercent.toFixed(1),
          (p) => data.interestRatePercent = parseFloat(p)
        }
      />
    </div>
    <Slider
      max={MAX_INTEREST_RATE_PERCENT}
      step={0.1}
      tickInterval={0.5}
      bind:value={data.interestRatePercent}
    />

    <label for="loan-term">Loan Term</label>
    <div class="field years">
      <input
        id="loan-term"
        type="number"
        max={MAX_LOAN_TERM}
        value={loanTermYears}
        disabled
      />
    </div>

    <label for="monthly-payment">Monthly payment</label>
    <div class="field dollar">
      <input
        id="monthly-payment"
        type="number"
        value={monthlyPayment}
        disabled
      />
    </div>
  </div>
</main>

<style>
  .calculator-grid {
    display: grid;
    grid-template-columns: 120px 4fr;
    gap: 0.5rem;
  }

  .calculator-grid label {
    grid-column: 1 / -1;
  }

  .calculator-grid input {
    max-width: 100%;
  }

  .field.dollar input {
    width: 100px;
  }
  .field.dollar:before {
    content: "$";
    margin-right: 3px;
  }

  .field.percentage input {
    width: 75px;
  }
  .field.percentage:after {
    content: "%";
    margin-left: 3px;
  }

  .field.years input {
    width: 50px;
  }
  .field.years:after {
    content: "years";
    margin-left: 3px;
  }
</style>
