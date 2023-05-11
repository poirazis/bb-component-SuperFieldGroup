<script>
  import { getContext, setContext } from "svelte"

  const { styleable } = getContext("sdk")
  const component = getContext("component")

  export let header
  export let labelPos
  export let labelWidth
  export let spacing
  export let collapsible
  export let columns

  setContext("field-group", { labelPos })
</script>

<div class="wrapper" use:styleable={$component.styles}>
  {#if header}
    <h4
      style:margin-bottom={spacing} 
      class="spectrum-Heading spectrum-Heading--sizeXS">{header}</h4>
  {/if}
  <div
    style:--spectrum-global-dimension-size-300={spacing}
    style:--spectrum-global-dimension-size-250={spacing}
    style:--label-column-width={labelWidth || "fit-contents"}
    class="spectrum-Form"
    class:spectrum-Form--labelsAbove={labelPos === "above"}
  >
    <slot />
  </div>
</div>

<style>
  .wrapper {
    width: 100%;
    position: relative;
  }

 :global(.spectrum-Form-itemLabel) {
    min-width: var(--label-column-width) !important;
  }
  .spectrum-Form {
    width: 100%;
  }
  .spectrum-Form--labelsAbove {
    gap: 2px !important;
  }
</style>
