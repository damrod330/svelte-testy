<script>
  import { beforeUpdate, afterUpdate } from "svelte";

  export let targets = [];
  export let html = "";

  let counter = 0;

  let startingItems = [];
  for (let i = 0; i <= 10; i++) {
    startingItems.push(`<li>Prosty element listy ${i}</li>`);
  }

  let items = startingItems;
  beforeUpdate(() => {
    let newItems = [...startingItems];
    targets.forEach(target => {
      newItems.splice(target, 1);
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
