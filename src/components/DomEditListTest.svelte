<script>
  import { beforeUpdate } from "svelte";

  export let targets = [];
  export let html = "";

  let counter = 0;

  let items = [];
  for (let i = 0; i <= 100; i++) {
    items.push(`<li><h3>Element listy H2 - ${i}</h3></li>`);
  }

  beforeUpdate(() => {
    let newItems = [...items];
    targets.forEach(target => {
      newItems[target] = `<li>${html}</li>`;
    });
    items = newItems;
  });
</script>

<style>
  ul {
    list-style: none;
  }

  li {
    margin-bottom: 4px;
    display: flex;
    flex-direction: row;
  }
</style>

<ul>
  {@html items.join('')}
</ul>
