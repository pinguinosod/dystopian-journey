<script>
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
      energyPoints: 100
    },
    {
      id: 3,
      name: "Android",
      healthPoints: 160,
      energyPoints: 100
    }
  ];

  const travelerBoilerplate = {
    name: "Traveler's Name",
    race: 1,
    level: 1,
    experience: 0
  };

  const party = [];

  let newTraveler = null;

  function createNewTraveler() {
    newTraveler = { ...travelerBoilerplate };
  }

  function addNewTravelerToParty() {
    const initialHp = getRaceInitialHp(newTraveler.race);
    newTraveler.healthPoints = { current: initialHp, max: initialHp };
    const initialEp = getRaceInitialEp(newTraveler.race);
    newTraveler.energyPoints = { current: initialEp, max: initialEp };
    party[party.length] = { ...newTraveler };
    newTraveler = null;
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
  .traveler {
    display: inline-block;
    border: 1px solid black;
    padding: 0 15px 15px 15px;
		margin: 5px;
		width: 230px;
		min-height: 180px;
  }
  .traveler.Human > h2 {
    color: darkred;
  }
  .traveler.Cyborg > h2 {
    color: purple;
  }
  .traveler.Android > h2 {
    color: darkblue;
  }
</style>

{#if gameOver}
  <h1>GAME OVER =(</h1>
{/if}

{#if newTraveler !== null}
	<h1>New Traveler</h1>
  <input type="text" bind:value={newTraveler.name} />
  <select bind:value={newTraveler.race}>
    {#each races as race}
      <option value={race.id}>{race.name}</option>
    {/each}
  </select>
  <button on:click={addNewTravelerToParty}>Add Traveler To Party</button>
  <br />
  <div class={'traveler ' + getRaceName(newTraveler.race)}>
    <h2>{newTraveler.name.toUpperCase()}</h2>
    <p>Race: {getRaceName(newTraveler.race)}</p>
  </div>
{:else if party.length < 3}
  <button on:click={createNewTraveler}>Create New Traveler</button>
{:else if !gameOver}
  <button on:click|once={startGame}>Embark!</button>
{/if}

<hr />

<h1>Your Party</h1>
{#if party.length < 3}
  <p>Add 3 travelers to your party and then embark on the journy</p>
{/if}
{#each party as traveler}
  <div class={'traveler ' + getRaceName(traveler.race)}>
    <h2>{traveler.name.toUpperCase()}</h2>
    <p>{getRaceName(traveler.race)} {traveler.level} ({traveler.experience})</p>
    <p>HP: {traveler.healthPoints.current}/{traveler.healthPoints.max}</p>
    <p>EP: {traveler.energyPoints.current}/{traveler.energyPoints.max}</p>
  </div>
{/each}
