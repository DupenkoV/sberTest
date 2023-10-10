<script lang="ts">
  let currenciesList:string[] = [];
  let conversion_rates:any = {};

  let first_currencie: string = 'RUB'
  let second_currencie: string = 'USD'
  let first_currencie_input: number = 0;
  let second_currencie_input: number = 0;

  function calc(type: string) {
    if(type === 'first') {
      return conversion_rates[first_currencie] > conversion_rates[second_currencie] ?  first_currencie_input * conversion_rates[second_currencie] : conversion_rates[first_currencie] / first_currencie_input
    } else  {
      return conversion_rates[first_currencie] < conversion_rates[second_currencie] ?  second_currencie_input * conversion_rates[first_currencie] : conversion_rates[second_currencie] / second_currencie_input
    }
  }
  function changeValue(event) {
    if(event.target.getAttribute('name') === 'first_currencie') {
      console.log(1)
      first_currencie_input = event.target.value;
      second_currencie_input = calc('first')
    } else {
      second_currencie_input = event.target.value;
      first_currencie_input = calc('second')
    }
  }
  $: {
    console.log(conversion_rates)
  }
  function fetchConversion(item:string) {
    fetch(`https://v6.exchangerate-api.com/v6/1e2df0d944881b36269ef760/latest/${item}`).then(res => res.json()).then(data => {
    conversion_rates = data.conversion_rates;
    currenciesList = Object.keys(data.conversion_rates);
  })
  }
  $: {
    fetchConversion(first_currencie);
  }


</script>

<main>
  <select name="first_currencie" id="first_currencie" bind:value={first_currencie} >
    $: {#each currenciesList as currencies}
    <option value={currencies} >{currencies}</option>
    {/each}
  </select>
  <input type="number" name="first_currencie" value={first_currencie_input} min={0} on:input={changeValue}>
  =
  <select name="second_currencie" id="second_currencie" bind:value={second_currencie}>
    $: {#each currenciesList as currencies}
    <option value={currencies}>{currencies}</option>
    {/each}
  </select>
  <input type="number" name="second_currencie" value={second_currencie_input} min={0} on:input={changeValue}>
</main>

<style>
  
</style>
