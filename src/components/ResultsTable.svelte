<script>
  import testList from "../testList.js";
  export let measures = [];

  $: resultList = getNewResultList(measures);

  function getCsvLink(data) {
    let csvContent = "data:text/csv;charset=utf-8,";
    data.forEach(function(result) {
      csvContent += result + ";" + "\r\n";
    });
    return encodeURI(csvContent);
  }

  function getNewResultList(measures) {
    if (measures) {
      let newResultsList = [];
      testList.forEach(test => {
        let resultsData = measures
          .filter(measure => {
            return measure.name.search(test.id) > -1 ? true : false;
          })
          .map(measure => {
            return measure.duration;
          });
        if (resultsData.length > 0)
          newResultsList.push({
            id: test.id,
            name: test.name,
            data: resultsData,
            downloadLink: getCsvLink(resultsData, test.id)
          });
      });
      return newResultsList;
    }
    return [];
  }
</script>

<section class="card">
  {#if resultList.length > 0}
    <table>
      <thead>
        <tr>
          <th>Test</th>
          <th>Ilość iteracji</th>
          <th>Pierwsze wykonanie (ms)</th>
          <th>Średni czas (ms)</th>
          <th>.csv</th>
        </tr>
      </thead>
      <tbody>
        {#each resultList as result}
          <tr key={result.id}>
            <td>{result.id}</td>
            <td>{result.data.length}</td>
            <td>{result.data.shift()}</td>
                <td>{result.data.length > 0 ? result.data.reduce((sum, item)=>{
                    return sum + item;
                }) / result.data.length : 'brak elementów'}</td>
            <td>
              <a href={result.downloadLink} download={result.id + '.csv'}>
                Pobierz
              </a>
            </td>
          </tr>
        {/each}
      </tbody>
    </table>
  {:else}
    <p>Nie znaleziono żadnych wyników.</p>
  {/if}
</section>
