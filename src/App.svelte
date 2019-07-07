<script>
  import Traveler from "./Traveler.svelte";
  import { quintOut } from "svelte/easing";
  import { crossfade } from "svelte/transition";
  import { flip } from "svelte/animate";

  const [send, receive] = crossfade({
    duration: d => Math.sqrt(d * 200),

    fallback(node, params) {
      const style = getComputedStyle(node);
      const transform = style.transform === "none" ? "" : style.transform;

      return {
        duration: 600,
        easing: quintOut,
        css: t => `
					transform: ${transform} scale(${t});
					opacity: ${t}
				`
      };
    }
  });

  let gameOver = false;

  const races = [
    {
      id: 1,
      name: "Human",
      healthPoints: 100,
      energyPoints: 100
    },
    {
      id: 2,
      name: "Cyborg",
      healthPoints: 130,
      energyPoints: 150
    },
    {
      id: 3,
      name: "Android",
      healthPoints: 160,
      energyPoints: 200
    }
  ];

  const travelerBoilerplate = {
    id: 0,
    selected: false,
    name: "Traveler's Name",
    race: 1,
    level: 1,
    experience: 0
  };

  $: {
    party.map(traveler => {
      if (traveler.selected) {
        const initialHp = getRaceInitialHp(traveler.race);
        traveler.healthPoints = { current: initialHp, max: initialHp };
        const initialEp = getRaceInitialEp(traveler.race);
        traveler.energyPoints = { current: initialEp, max: initialEp };
      }
    });
  }

  const party = [];

  function createNewTraveler() {
    const newTraveler = { ...travelerBoilerplate };
    newTraveler.id = party.length + 1;
    newTraveler.selected = true;
    party[party.length] = newTraveler;
  }

  function addNewTravelerToParty(id) {
    const traveler = party.filter(t => t.id === id)[0];
    traveler.selected = false;
    party = party;
  }

  function getRaceName(id) {
    return races.filter(race => race.id === id)[0].name;
  }

  function getRaceInitialHp(id) {
    return races.filter(race => race.id === id)[0].healthPoints;
  }

  function getRaceInitialEp(id) {
    return races.filter(race => race.id === id)[0].energyPoints;
  }

  function startGame() {
    alert("not developed yet =(");
    gameOver = true;
  }
</script>

<style>
  div {
    display: inline-block;
  }
</style>

{#if !gameOver}
  <h1>Dystopian Journey</h1>
{:else}
  <h1>GAME OVER =(</h1>
{/if}

{#each party.filter(t => t.selected) as traveler (traveler.id)}
  <div animate:flip>
    <input type="text" bind:value={traveler.name} />
    <select bind:value={traveler.race}>
      {#each races as race}
        <option value={race.id}>{race.name}</option>
      {/each}
    </select>
    <button on:click={() => addNewTravelerToParty(traveler.id)}>
      Add Traveler To Party
    </button>
    <br />
    <div out:send={{ key: traveler.id }} in:receive={{ key: traveler.id }}>
      <Traveler {...traveler} {getRaceName} />
    </div>
  </div>
{/each}

{#if party.filter(t => t.selected).length === 0}
  {#if party.length < 3}
    <button on:click={createNewTraveler}>Create New Traveler</button>
  {:else if !gameOver}
    <button on:click|once={startGame}>Embark!</button>
  {/if}
{/if}

<hr />

<h2>Your Party</h2>
{#if party.length < 3}
  <p>Add 3 travelers to your party and then embark on the journey</p>
{/if}
{#each party.filter(t => !t.selected) as traveler (traveler.id)}
  <div
    out:send={{ key: traveler.id }}
    in:receive={{ key: traveler.id }}
    animate:flip>
    <Traveler {...traveler} {getRaceName} />
  </div>
{/each}
