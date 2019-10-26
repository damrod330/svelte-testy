<script>
  import { beforeUpdate } from "svelte";

  export let html = null;
  let items = [];

  beforeUpdate(() => {
    if (html) {
      let newItems = [...items, { html: html }];
      items = newItems;
    }
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
  {#each items as item}
    <li>
      {@html item.html}
    </li>
  {/each}
</ul>
