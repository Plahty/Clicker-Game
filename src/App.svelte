<script>
  import { onMount, beforeUpdate } from 'svelte';

  $: document.title = `${formatNumber(count)} Clicks`;

  let count = 0;
  let totalCPS = 0;
  let clickValue = 1;

  let buttons = [
    { id: 1, cost: 15, cps: 0.2, owned: 0 },
    { id: 2, cost: 100, cps: 1, owned: 0 },
    { id: 3, cost: 625, cps: 5, owned: 0 },
    { id: 4, cost: 3000, cps: 20, owned: 0 },
    { id: 5, cost: 9625, cps: 55, owned: 0 },
    { id: 6, cost: 39000, cps: 195, owned: 0 },
    { id: 7, cost: 123750, cps: 550, owned: 0 },
    { id: 8, cost: 437500, cps: 1750, owned: 0 },
    { id: 9, cost: 1787500, cps: 6500, owned: 0 },
    { id: 10, cost: 9999900, cps: 33333, owned: 0 },
  ];

  let upgradeClick = { cost: 250, costMultiplier: 2, clickMultiplier: 2 };
  let upgradeMultiplier2 = { cost: 950, costMultiplier: 1.5, cpsMultiplier: 1.00 };
  let upgradeCost = { cost: 1750, costMultiplier: 1.5, costReduction: 0.00 };

  function roundToHundredths(value) {
    return Math.round(value * 100) / 100;
  }

  function formatNumber(value) {
    let formattedValue = value.toFixed(2);
    if (formattedValue.endsWith('.00')) {
      formattedValue = formattedValue.substring(0, formattedValue.length - 3);
    }
    return formattedValue.replace(/\B(?=(\d{3})+(?!\d))/g, ",");
  }

  beforeUpdate(() => {
    count = roundToHundredths(count);
    totalCPS = roundToHundredths(totalCPS);
    clickValue = roundToHundredths(clickValue);
    upgradeClick.cost = roundToHundredths(upgradeClick.cost);
    upgradeMultiplier2.cost = roundToHundredths(upgradeMultiplier2.cost);
    buttons = buttons.map(button => ({
      ...button,
      cost: roundToHundredths(button.cost),
    }));
  });

  function increment() {
    count += clickValue;
  }

  function upgradeClicker() {
    if (count >= upgradeClick.cost) {
      count -= upgradeClick.cost;
      clickValue *= upgradeClick.clickMultiplier;
      upgradeClick.cost *= upgradeClick.costMultiplier;
    }
  }

  function upgradeCps() {
  if (count >= upgradeMultiplier2.cost) {
    count -= upgradeMultiplier2.cost;
    upgradeMultiplier2.cost *= upgradeMultiplier2.costMultiplier;
    upgradeMultiplier2.cpsMultiplier += 0.02; // Increment cpsMultiplier
    buttons = buttons.map(button => ({
      ...button,
      cps: button.cps * upgradeMultiplier2.cpsMultiplier,
    }));
  }
}

function upgradeCostFunc() {
    if (count >= upgradeCost.cost) {
      count -= upgradeCost.cost;
      upgradeCost.cost *= upgradeCost.costMultiplier; // Re-add this line
      upgradeCost.costReduction += 0.01; // Increment costReduction
      buttons = buttons.map(button => ({
        ...button,
        cost: button.cost * (1 - upgradeCost.costReduction),
      }));
    }
  }

  function buyButton1() {
    let button = buttons[0];
    if (count >= button.cost) {
      count -= button.cost;
      button.owned += 1;
      button.cost *= 1.15;
    }
  }

  function buyButton2() {
    let button = buttons[1];
    if (count >= button.cost) {
      count -= button.cost;
      button.owned += 1;
      button.cost *= 1.15;
    }
  }

  function buyButton3() {
    let button = buttons[2];
    if (count >= button.cost) {
      count -= button.cost;
      button.owned += 1;
      button.cost *= 1.15;
    }
  }

  function buyButton4() {
    let button = buttons[3];
    if (count >= button.cost) {
      count -= button.cost;
      button.owned += 1;
      button.cost *= 1.15;
    }
  }

  function buyButton5() {
    let button = buttons[4];
    if (count >= button.cost) {
      count -= button.cost;
      button.owned += 1;
      button.cost *= 1.15;
    }
  }

  function buyButton6() {
    let button = buttons[5];
    if (count >= button.cost) {
      count -= button.cost;
      button.owned += 1;
      button.cost *= 1.15;
    }
  }

  function buyButton7() {
    let button = buttons[6];
    if (count >= button.cost) {
      count -= button.cost;
      button.owned += 1;
      button.cost *= 1.15;
    }
  }

  function buyButton8() {
    let button = buttons[7];
    if (count >= button.cost) {
      count -= button.cost;
      button.owned += 1;
      button.cost *= 1.15;
    }
  }

  function buyButton9() {
    let button = buttons[8];
    if (count >= button.cost) {
      count -= button.cost;
      button.owned += 1;
      button.cost *= 1.15;
    }
  }

  function buyButton10() {
    let button = buttons[9];
    if (count >= button.cost) {
      count -= button.cost;
      button.owned += 1;
      button.cost *= 1.15;
    }
  }

  onMount(() => {
    let interval = setInterval(() => {
      totalCPS = buttons.reduce((sum, button) => sum + button.owned * button.cps, 0);
      count += totalCPS / 10;
    }, 100);

    return () => {
      clearInterval(interval);
    };
  });

</script>

<div class="container">
  <div id="buttons-left">
    <h2>Upgrades</h2>
    <button on:click={upgradeClicker} class="upgradeButton">Upgrade Clicker (Cost: {formatNumber(upgradeClick.cost)}) Power: {formatNumber(clickValue)}</button>
    <button on:click={upgradeCps} class="upgradeButton">Increase CPS +2% (Cost: {formatNumber(upgradeMultiplier2.cost)}) Multiplier: +{Math.round((upgradeMultiplier2.cpsMultiplier - 1) * 100)}%</button>
    <button on:click={upgradeCostFunc} class="upgradeButton">Reduce Costs -1% (Cost: {formatNumber(upgradeCost.cost)}) Reduction: -{Math.round(upgradeCost.costReduction * 100)}%</button>
  </div>

  <div class="center-content">
    <h1 class="title">Clicker Game</h1>
    <h2 class="cps">{formatNumber(totalCPS)} CPS</h2>
    <h3 class="count">{formatNumber(count)} Clicks</h3>
    <button class="click-me" on:click={increment}>Click me</button>
</div>

  <div id="buttons-right">
    <h2>Store</h2>
    <button on:click={buyButton1} class="button1">Clicker 1 (Cost: {formatNumber(buttons[0].cost)}, CPS: {formatNumber(buttons[0].cps)}, Owned: {buttons[0].owned})</button>
    <button on:click={buyButton2} class="button2">Clicker 2 (Cost: {formatNumber(buttons[1].cost)}, CPS: {formatNumber(buttons[1].cps)}, Owned: {buttons[1].owned})</button>
    <button on:click={buyButton3} class="button3">Clicker 3 (Cost: {formatNumber(buttons[2].cost)}, CPS: {formatNumber(buttons[2].cps)}, Owned: {buttons[2].owned})</button>
    <button on:click={buyButton4} class="button4">Clicker 4 (Cost: {formatNumber(buttons[3].cost)}, CPS: {formatNumber(buttons[3].cps)}, Owned: {buttons[3].owned})</button>
    <button on:click={buyButton5} class="button5">Clicker 5 (Cost: {formatNumber(buttons[4].cost)}, CPS: {formatNumber(buttons[4].cps)}, Owned: {buttons[4].owned})</button>
    <button on:click={buyButton6} class="button6">Clicker 6 (Cost: {formatNumber(buttons[5].cost)}, CPS: {formatNumber(buttons[5].cps)}, Owned: {buttons[5].owned})</button>
    <button on:click={buyButton7} class="button7">Clicker 7 (Cost: {formatNumber(buttons[6].cost)}, CPS: {formatNumber(buttons[6].cps)}, Owned: {buttons[6].owned})</button>
    <button on:click={buyButton8} class="button8">Clicker 8 (Cost: {formatNumber(buttons[7].cost)}, CPS: {formatNumber(buttons[7].cps)}, Owned: {buttons[7].owned})</button>
    <button on:click={buyButton9} class="button9">Clicker 9 (Cost: {formatNumber(buttons[8].cost)}, CPS: {formatNumber(buttons[8].cps)}, Owned: {buttons[8].owned})</button>
    <button on:click={buyButton10} class="button10">Clicker 10 (Cost: {formatNumber(buttons[9].cost)}, CPS: {formatNumber(buttons[9].cps)}, Owned: {buttons[9].owned})</button>
  </div>
</div>

<style>
  .container {
    display: flex;
    justify-content: center;
    align-items: flex-start;
    height: 150vh;
    padding: 1rem;
  }

  .center-content {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 1rem;
  }

  .title {
    font-size: 2.5rem;
    margin: 1rem;
    min-width: 16rem;
  }

  .cps {
    font-size: 1.2rem;
    margin: 0rem;
    min-width: 15rem;
  }

  .count{
    font-size: 1.2rem;
    margin: 1rem;
    min-width: 15rem;
  }

  .click-me{
    margin: 1rem;
    background-color: #dfdfdf;
  }

  .upgradeButton {
    width: 30rem;
    border: 1px solid black;
    padding: 0.5rem;
    background-color: #dfdfdf;
    margin-bottom: 0.5rem;
  }

  .button1 {
    width: 30rem;
    border: 1px solid black;
    padding: 0.5rem;
    background-color: #bddbc2;
    margin-bottom: 0.5rem;
  }

  .button2 {
    width: 30rem;
    border: 1px solid black;
    padding: 0.5rem;
    background-color: #ccdbbd;
    margin-bottom: 0.5rem;
  }

  .button3 {
    width: 30rem;
    border: 1px solid black;
    padding: 0.5rem;
    background-color: #d9dbbd;
    margin-bottom: 0.5rem;
  }

  .button4 {
    width: 30rem;
    border: 1px solid black;
    padding: 0.5rem;
    background-color: #dbd3bd;
    margin-bottom: 0.5rem;
  }

  .button5 {
    width: 30rem;
    border: 1px solid black;
    padding: 0.5rem;
    background-color: #dbbebd;
    margin-bottom: 0.5rem;
  }

  .button6 {
    width: 30rem;
    border: 1px solid black;
    padding: 0.5rem;
    background-color: #dbbdca;
    margin-bottom: 0.5rem;
  }

  .button7 {
    width: 30rem;
    border: 1px solid black;
    padding: 0.5rem;
    background-color: #dabddb;
    margin-bottom: 0.5rem;
  }

  .button8 {
    width: 30rem;
    border: 1px solid black;
    padding: 0.5rem;
    background-color: #c2bddb;
    margin-bottom: 0.5rem;
  }

  .button9 {
    width: 30rem;
    border: 1px solid black;
    padding: 0.5rem;
    background-color: #779cb8;
    margin-bottom: 0.5rem;
  }

  .button10 {
    width: 30rem;
    border: 1px solid black;
    padding: 0.5rem;
    background-color: #4a8b93;
    margin-bottom: 0.5rem;
  }

  #buttons-left,
  #buttons-right {
    display: flex;
    flex-direction: column;
    align-self: flex-start;
    padding: 2rem;
    margin-top: 3rem;
  }

  h1 {
    font-size: 2rem;
  }

  h3 {
    font-size: 1rem;
  }

  :global(body) {
  background-color: #ffffff;
  }

  * {

  }

</style>