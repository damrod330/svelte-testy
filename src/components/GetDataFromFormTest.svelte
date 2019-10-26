<script>
  import { afterUpdate } from "svelte";
  export let username;
  export let password;

  afterUpdate(() => {
    console.log("Username: " + username + "/n Password: " + password);
  });
</script>

<form>
  <div class="form-control">
    <label>Login:</label>
    <input type="text" placeholder="login" bind:value={username} />
  </div>
  <div class="form-control">
    <label>Hasło:</label>
    <input type="password" placeholder="hasło" bind:value={password} />
  </div>
  <button type="submit" id="T07-submit">Zaloguj się</button>
</form>
