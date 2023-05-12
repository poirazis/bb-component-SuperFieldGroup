<script>
  import { getContext, setContext } from "svelte"
  import Placeholder from "./Placeholder.svelte";

  const { styleable } = getContext("sdk")
  const component = getContext("component")

  export let hideHeader
  export let header
  export let headerSize
  export let headerBold
  export let headerFontColor = "var(--spectrum-global-color-gray-700)"
  export let headerIcon
  export let labelPos
  export let labelWidth
  export let labelSize
  export let labelWeight
  export let rowSpacing
  export let columnSpacing
  export let collapsible = true
  export let collapsed = false
  export let columns
  export let disabled

  setContext("field-group", { labelPos })
</script>

<div class="wrapper" use:styleable={$component.styles}>

  {#if collapsible}
  <div class="spectrum-Accordion">
    <div class:is-disabled={disabled} class:is-open={!collapsed} class="spectrum-Accordion-item">
      <h3 
        style:--header-font-size={headerSize} 
        style:--header-font-color={headerFontColor} 
        style:--header-font-bold={headerBold} 
        class="spectrum-Accordion-itemHeading">
        <button on:click={() => { if (!disabled) collapsed = !collapsed }} class="spectrum-Accordion-itemHeader" type="button">{header}</button>
        <svg class="spectrum-Icon spectrum-UIIcon-ChevronRight100 spectrum-Accordion-itemIndicator" focusable="false" aria-hidden="true">
          <use xlink:href="#spectrum-css-icon-Chevron100" />
        </svg>
      </h3>
        <div class="spectrum-Accordion-itemContent" role="region">
          <div
            class="spectrum-Form"
            class:spectrum-Form--labelsAbove={labelPos === "above"}
            style:--grid-row-gap={rowSpacing}
            style:--grid-column-gap={columnSpacing}
            style:--grid-columns={columns}
            style:--label-column-width={labelWidth || "fit-contents"}
            style:--spectrum-tableform-margin={"unset"}
            style:--spectrum-tableform-border-spacing={"unset"}
            style:--label-font-size={labelSize}
            style:--label-font-weight={labelWeight}

          >   
            <slot />
          </div>
        </div>
    </div>
  </div>
  {:else}
    {#if !hideHeader}
      <h6
        class="spectrum-Heading"
        style:margin-bottom={rowSpacing}
        style:--header-font-size={headerSize}
        style:--header-font-color={headerFontColor} 
        style:--header-font-bold={headerBold ? "700" : "500"} 
        >
        {header.toUpperCase()}
      </h6>
    {/if}
    <div
      class="spectrum-Form"
      class:spectrum-Form--labelsAbove={labelPos === "above"}
      style:--grid-row-gap={rowSpacing}
      style:--grid-column-gap={columnSpacing}
      style:--grid-columns={columns}
      style:--label-column-width={labelWidth || "fit-contents"}
      style:--spectrum-tableform-margin={"unset"}
      style:--spectrum-tableform-border-spacing={"unset"}
      style:--label-font-size={labelSize}
      style:--label-font-weight={labelWeight}

    >   
      <slot />
    </div>
    {/if}
  </div>



<style>
  .wrapper {
    width: 100%;
    position: relative;
  }

 :global(.spectrum-Form-itemLabel) {
    min-width: var(--label-column-width) !important;
    font-size: var(--label-font-size) !important;
    font-weight: var(--label-font-weight) !important;
    padding-right: 0.85rem;
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
  .spectrum-Heading {
    color: var(--header-font-color) !important;
    font-weight: var(--header-font-bold) !important;
    font-size: var(--header-font-size) !important;
  }

  .spectrum-Accordion-itemHeading .spectrum-Accordion-itemHeader {
    color: var(--header-font-color) !important;
    font-size: var(--header-font-size) !important;
    font-weight: var(--header-font-bold) !important;
    min-height: unset !important;
  }
</style>
