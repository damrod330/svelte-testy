<script>
  import { beforeUpdate, afterUpdate, onMount } from "svelte";

  export let id;
  export let Component;
  export let props;

  let mounted = false;

  beforeUpdate(() => {
    window.performance.mark(mounted ? `${id}UpdateStart` : `${id}MountStart`);
  });

  afterUpdate(() => {
    if (mounted) {
      window.performance.mark(`${id}UpdateEnd`);
      window.performance.measure(
        `${id}Update`,
        `${id}UpdateStart`,
        `${id}UpdateEnd`
      );
    } else {
      window.performance.mark(`${id}MountEnd`);
      window.performance.measure(
        `${id}Mount`,
        `${id}MountStart`,
        `${id}MountEnd`
      );
      mounted = true;
    }
  });
</script>

<Component {...props} />
