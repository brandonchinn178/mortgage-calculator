<script lang="ts">
  import Slider from "$lib/Slider.svelte"

  const MAX_TOTAL_HOUSE = 5000000

  // TODO:
  //   * down payment
  //   * loan term
  //   * interest rate
  //   * tax
  //   * insurance
  //   * HOA
  //   * monthly payment
  let data = $state({
    totalHouse: 500000,
  })

  // TODO: outputs: monthly payment, down payment
</script>

<header>
  <h1>Interactive Mortgage Calculator</h1>
</header>
<main>
  <div class="calculator-grid">
    <label for="total-house">Total house cost</label>
    <input
      id="total-house"
      type="number"
      max={MAX_TOTAL_HOUSE}
      bind:value={data.totalHouse}
    />
    <Slider
      max={MAX_TOTAL_HOUSE}
      tickInterval={250000}
      bind:value={data.totalHouse}
    />
  </div>
</main>

<style>
  .calculator-grid {
    display: grid;
    grid-template-columns: 1fr 4fr;
    gap: 0.5rem;
  }

  .calculator-grid label {
    grid-column: 1 / -1;
  }
</style>
