<script>
  import Button from "sveltekit-ui/Button/index.svelte"
  import {
    openModalTrigger, isSignedIn
  } from "$lib/components/Auth/store";
</script>

{#if !$isSignedIn}
  <Button type="primary" on:click={() => $openModalTrigger++}>
    <h5>Sign In</h5>
  </Button>
{/if}
