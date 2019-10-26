<script>
  import ResultsTable from "./components/ResultsTable.svelte";
  import testList from "./testList.js";
  import DomTextTest from "./components/DomTextTest.svelte";
  import MeasureRenderer from "./components/MeasureRenderer.svelte";

  let currentTestId = "T01",
    currentComponentInstance = null,
    repeatAmount = 1,
    timer = 200,
    oddProp = false,
    interval = null;

  $: measures = [];

  function handleSelect(e) {
    currentTestId = e.target.value;
    currentComponentInstance ? currentComponentInstance.$destroy() : false;
    currentComponentInstance = null;
  }

  function handleRunTestClicked() {
    if (interval) {
      clearInterval(interval);
      interval = null;
      return "";
    }
    let test = testList.find(test => test.id === currentTestId);
    if (timer) {
      interval = setInterval(() => {
        runTest(test);
      }, timer);
    } else {
      runTest(test);
    }
  }

  function runTest(test, count) {
    oddProp = !oddProp;
    if (!currentComponentInstance) {
      // create new instance
      currentComponentInstance = new MeasureRenderer({
        props: {
          id: test.id,
          Component: test.run,
          props: test.props[oddProp ? 0 : 1]
        },
        target: document.getElementById("test-area")
      });
    } else {
      currentComponentInstance.$set({ props: test.props[oddProp ? 0 : 1] });
    }
    updateResults();
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
          <option value={test.id} key={test.id}>[{test.id}] {test.name}</option>
        {/each}
      </select>
    </div>
    <div class="form-control">
      <label>Ilość powtórzeń:</label>
      <input
        type="number"
        name="select-test"
        value={repeatAmount}
        placeholder="Ilość powtórzeń" />
    </div>
    <div class="form-control">
      <button type="button" on:click={handleRunTestClicked}>
        {!interval ? 'Uruchom' : 'Zatrzymaj'}
      </button>
    </div>
  </section>
  <ResultsTable {measures} />
  <section class="card" id="test-area" />

</main>
