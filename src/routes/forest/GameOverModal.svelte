<script>
  export let show; // Export the 'show' prop

  // You can add additional logic here if needed

  // Function to close the modal
  function closeModal() {
    show = false;
  }
  let dialog; // HTMLDialogElement

  $: if (dialog && show) dialog.showModal();
</script>

<!-- svelte-ignore a11y-click-events-have-key-events a11y-no-noninteractive-element-interactions -->
<dialog
  bind:this={dialog}
  on:close={() => (show = false)}
  on:click|self={() => dialog.close()}
>
  <!-- svelte-ignore a11y-no-static-element-interactions -->
  <div on:click|stopPropagation>
    <slot name="header" />
    <h2>Congratulations! You've found all the items!</h2>
    <slot />
    <hr />
    <!-- svelte-ignore a11y-autofocus -->
    <button autofocus on:click={() => dialog.close()}>close modal</button>
  </div>
</dialog>
