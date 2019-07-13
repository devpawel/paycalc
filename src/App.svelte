<script>
  import { onMount } from "svelte";

  let rate = 0;
  let from = 0;
  let stupidEUTax = 1.3;
  let vat = 0.77;
  let incomeTax = 0.81;
  $: to = from * rate * stupidEUTax * vat * incomeTax || 0;
  $: withTax = from * rate * stupidEUTax || 0;

  let fromPLN = 0;
  $: toPLN = fromPLN * vat * incomeTax || 0;

  onMount(async () => {
    rate = await fetch(
      "https://api.exchangeratesapi.io/latest?base=USD&symbols=PLN"
    )
      .then(res => res.json())
      .then(data => data.rates.PLN);
  });
</script>

<style>
  .calc {
    width: 430px;
    border: 1px solid var(--primary-color);
    padding: 1rem;
    margin: 1rem;
  }
  .input {
    width: 100%;
    background-color: var(--bg-color);
    color: var(--primary-color);
  }
  .title {
    border-bottom: 2px solid var(--primary-color);
  }
  [class*="col"] {
    line-height: 1.5rem;
  }
</style>

<link
  rel="stylesheet"
  href="https://cdnjs.cloudflare.com/ajax/libs/flexboxgrid/6.3.1/flexboxgrid.min.css"
  type="text/css" />
<link
  href="https://fonts.googleapis.com/css?family=Roboto&display=swap"
  rel="stylesheet" />

<div class="container">
  <div class="row around-xs">
    <div class="usd-to-pln calc">
      <h3 class="title">USD to PLN</h3>

      <div class="row">
        <div class="col-xs-6">Stupid EU Tax:</div>
        <div class="col-xs-6">
          <input
            class="input"
            type="number"
            min="0"
            step="0.01"
            lang="en-150"
            bind:value={stupidEUTax} />
        </div>

        <div class="col-xs-6">USD:</div>
        <div class="col-xs-6">
          <input
            class="input"
            type="number"
            min="0"
            step="0.01"
            lang="en-150"
            bind:value={from} />
        </div>

        <div class="col-xs-6">Rate:</div>
        <div class="col-xs-6">
          {#if rate === 0}Loading...{:else}{rate}{/if}
        </div>

        <div class="col-xs-6">PLN with tax:</div>
        <div class="col-xs-6">{withTax.toFixed(2)}</div>

        <div class="col-xs-6">PLN without taxes:</div>
        <div class="col-xs-6">{to.toFixed(2)}</div>
      </div>
    </div>

    <div class="pln calc">
      <h3 class="title">PLN</h3>

      <div class="row">
        <div class="col-xs-6">PLN:</div>
        <div class="col-xs-6">
          <input
            class="input"
            type="number"
            min="0"
            step="0.01"
            lang="en-150"
            bind:value={fromPLN} />
        </div>

        <div class="col-xs-6">PLN without taxes:</div>
        <div class="col-xs-6">{toPLN.toFixed(2)}</div>
      </div>
    </div>
  </div>
</div>
