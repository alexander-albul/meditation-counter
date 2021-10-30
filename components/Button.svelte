<script>
  export let title = "Button";
  export let secondary;
</script>



<button on:click class:secondary={secondary}>{title}</button>



<style>
  button {
    padding-inline: 1rem;
    padding-block: 0.625rem;
    font-size: 1rem;
    border: none;
    border-radius: 0.25rem;
    opacity: 0.9;
    user-select: none;
  }

  .secondary {
    color: white;
    background: transparent;
    box-shadow: 0 0 0 1px var(--semi-transparent);
  }
</style>