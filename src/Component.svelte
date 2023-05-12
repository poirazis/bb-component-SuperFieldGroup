<script>
  import { getContext, setContext } from "svelte"
  import Placeholder from "./Placeholder.svelte";

  const { styleable, builderStore } = getContext("sdk")
  const component = getContext("component")

  export let showHeader
  export let header
  export let headerSize
  export let headerBold
  export let labelPos
  export let labelWidth
  export let labelSize
  export let labelBold
  export let rowSpacing
  export let columnSpacing
  export let collapsible
  export let columns

  setContext("field-group", { labelPos })
</script>

<div class="wrapper" use:styleable={$component.styles}>
  {#if showHeader && header != "" }
    <h4
      style:margin-bottom={rowSpacing} 
      class="spectrum-Heading spectrum-Heading--size{headerSize}"
      class:spectrum-Heading--light={!headerBold}
      >{header}</h4>
  {/if}
  <div
    style:--grid-row-gap={rowSpacing}
    style:--grid-column-gap={columnSpacing}
    style:--grid-columns={columns}
    style:--label-column-width={labelWidth || "fit-contents"}
    style:--spectrum-tableform-margin={"unset"}
    style:--spectrum-tableform-border-spacing={"unset"}
    class="spectrum-Form"
    class:spectrum-Form--labelsAbove={labelPos === "above"}
  >
    <slot />
    {#if columns - $component.children > 0}
    {#each new Array(columns - $component.children) as _}
      <div class:placeholder={$builderStore.inBuilder}>
        <Placeholder />
      </div>
    {/each}
  {/if}
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
    display: grid;
    grid-template-columns: repeat(var(--grid-columns), 1fr [col-start]);
    column-gap: var(--grid-column-gap);
    row-gap: var(--grid-row-gap);
  }
  .spectrum-Form--labelsAbove {
    display: grid !important;
  }
</style>
