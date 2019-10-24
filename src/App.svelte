<script>
  import ResultsTable from "./components/ResultsTable.svelte";
  import testList from "./testList.js";
  import DomTextTest from "./components/DomTextTest.svelte";
  import MeasureRenderer from "./components/MeasureRenderer.svelte";

  let currentTestId = "T01",
    repeatAmount = 1,
    currentComponentInstance = null,
    oddProp = false;

    $: measures = [];

  function handleChange(e) {
    repeatAmount = e.target.value;
  }

  function handleSelect(e) {
    currentTestId = e.target.value;
    currentComponentInstance ? currentComponentInstance.$destroy() : false;
    currentComponentInstance = null;
  }

  function handleRunTestClicked() {
    let test = testList.find(test => test.id === currentTestId);
    let count = repeatAmount;
    while (count) {
      runTest(test, count);
      count--;
    }
    updateResults();
  }

  function runTest(test, repeatAmount) {
    oddProp = !oddProp;
    if (!currentComponentInstance) {
      // create new instance
      currentComponentInstance = new MeasureRenderer({
        props: {
          id: test.id,
          Component: test.component,
          props: test.props[oddProp ? 0 : 1]
        },
        target: document.getElementById("test-area")
      });
    } else {
      currentComponentInstance.$set({props: test.props[oddProp ? 0 : 1]});
    }
    return true;
  }

  function updateResults() {
    measures = window.performance.getEntriesByType("measure");
  }
</script>

<main class="container">
  <h2>Testy dla Svelte</h2>
  <section class="card">
    <div class="form-control">
      <label>Wybierz test:</label>
      <select name="select-test" value={currentTestId} on:change={handleSelect}>
        {#each testList as test}
          <option value={test.id} key={test.id}>{test.name}</option>
        {/each}
      </select>
    </div>
    <div class="form-control">
      <label>Ilość powtórzeń:</label>
      <input
        type="number"
        name="select-test"
        value={repeatAmount}
        on:change={handleChange}
        placeholder="Ilość powtórzeń" />
    </div>
    <div class="form-control">
      <button type="button" on:click={handleRunTestClicked}>Uruchom</button>
    </div>
  </section>
  <ResultsTable {measures} />
  <section class="card" id="test-area" />

</main>
