<script>
  export let show; // Export the 'show' prop

  let dialog; // HTMLDialogElement

  function closeModal() {
    show = false;
    dialog.close();
  }

  $: if (dialog && show) dialog.showModal();
</script>

<!-- svelte-ignore a11y-click-events-have-key-events a11y-no-noninteractive-element-interactions -->
<dialog
  bind:this={dialog}
  on:close={closeModal}
  on:click|self={() => dialog.close()}
>
  <!-- svelte-ignore a11y-no-static-element-interactions -->
  <div class="dialog-content">
    <button class="close-button" on:click={closeModal}>
      <span class="close-icon"><b>✕</b></span>
    </button>
    <slot name="header" />
    <h2>Congratulations! You've found all the items!</h2>
    <slot />
  </div>
</dialog>

<style>
  .dialog-content {
    padding: 1em;
    position: relative;
    margin-left: 1%;
    margin-bottom: 1%;
  }
  .close-button {
    position: absolute;
    top: 1em;
    right: 1em;
    background: red; /* Set the background color to yellow */
    border: none;
    border-radius: 50%; /* Make it a circle */
    width: 3.5em; /* Adjust the width and height as needed */
    height: 3.5em;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
  }
  .close-icon {
    font-size: 2.5em;
    color: orange;
  }
  .close-button:focus {
    outline: none;
  }
  .close-button:hover .close-icon {
    text-shadow: 0 0 5px rgba(255, 0, 0, 0.7);
  }
  .close-button:active .close-icon {
    transform: scale(1.1);
  }
  dialog[open] {
    animation: zoom 0.3s cubic-bezier(0.5, 2, 1, 1);
  }
  @keyframes zoom {
    from {
      transform: scale(0.95);
    }
    to {
      transform: scale(1);
    }
  }
  dialog[open]::backdrop {
    animation: fade 0.2s ease-out;
  }
  @keyframes fade {
    from {
      opacity: 0;
    }
    to {
      opacity: 1;
    }
  }
  button {
    display: block;
  }
</style>
