<script>
  export let name;
  export let race;
  export let level;
  export let experience;
  export let healthPoints;
  export let energyPoints;
  export let getRaceName;
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
  .traveler.Human > h3 {
    color: darkred;
  }
  .traveler.Cyborg > h3 {
    color: purple;
  }
  .traveler.Android > h3 {
    color: darkblue;
  }
</style>

<div class={'traveler ' + getRaceName(race)}>
  <h3>{name.toUpperCase()}</h3>
  <p>{getRaceName(race)} {level} ({experience})</p>
  <p>HP: {healthPoints.current}/{healthPoints.max}</p>
  <p>EP: {energyPoints.current}/{energyPoints.max}</p>
</div>
