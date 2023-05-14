<script>
  import { getContext, setContext } from "svelte"

  const { styleable } = getContext("sdk")
  const component = getContext("component")

  export let hideHeader
  export let header
  export let headerSize
  export let headerFontBold
  export let headerFontColor = "var(--spectrum-global-color-gray-700)"
  export let labelPos
  export let labelWidth
  export let labelSize
  export let labelWeight
  export let rowSpacing
  export let columnSpacing
  export let collapsible
  export let collapsed
  export let padding
  export let columns
  export let disabled

  export let column1 = "50%"

  let rowSpacingMap = {
    "above" : { XS : 1, S: 4, M : 8, L : 16 },
    "left" : { XS : 8, S: 16, M : 24, L : 32 },
    "right" : { XS : 8, S: 16, M : 24, L : 32 }
  }

  let colSpacingMap = {
    "above" : { XS : 4, S: 8, M : 16, L : 24 },
    "left" : { XS : 4, S: 8, M : 16, L : 24 },
  }

  $: gridColumnsDef = genmerateColumns( columns, column1 )

  function genmerateColumns ( ) {
    let columnsTemplate

    if (columns == 2 && column1 != "") 
      columnsTemplate = column1 + " auto" 
    else 
      columnsTemplate = "repeat(" + columns + ", 1fr )";

    return columnsTemplate
  }

  setContext("field-group", { labelPos })
</script>

<div class="wrapper" use:styleable={$component.styles}>
  {#if collapsible}
  <div class="spectrum-Accordion">
    <div class:is-disabled={disabled} class:is-open={!collapsed} class="spectrum-Accordion-item">
      <h3 
        style:--header-font-size={headerSize} 
        style:--header-font-color={headerFontColor} 
        style:--header-font-bold={headerFontBold ? "800" : "600"}
        class="spectrum-Accordion-itemHeading">
        <button 
          style:padding-left={padding ? "2.5rem" : "1.5rem"}
          on:click={() => { if (!disabled) collapsed = !collapsed }} 
          class="spectrum-Accordion-itemHeader" type="button">{header}
        </button>
        <svg
        style:left={padding ? "1rem" : "0px"}
        class="spectrum-Icon spectrum-UIIcon-ChevronRight100 spectrum-Accordion-itemIndicator" focusable="false" aria-hidden="true">
          <use xlink:href="#spectrum-css-icon-Chevron100" />
        </svg>
      </h3>
        <div 
          class="spectrum-Accordion-itemContent"
          style:padding-right={padding ? "1rem" : "0rem"} 
          style:padding-left={padding ? "1rem" : "0rem"} 
          role="region">
          <div
            class="spectrum-Form"
            class:spectrum-Form--labelsAbove={labelPos === "above"}
            style:--grid-row-gap={rowSpacingMap[labelPos][rowSpacing] + "px"}
            style:--grid-column-gap={colSpacingMap[labelPos][columnSpacing] + "px"}
            style:--grid-columns={gridColumnsDef}
            style:--label-column-width={labelWidth || "100px"}
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
        style:--header-font-size={headerSize}
        style:--header-font-color={headerFontColor} 
        style:--header-font-bold={headerFontBold ? "800" : "600"} 
        style:margin-bottom={"0.85rem"}
        >
        {header.toUpperCase()}
      </h6>
    {/if}
    <div
      class="spectrum-Form"
      class:spectrum-Form--labelsAbove={labelPos === "above"}
      style:--grid-row-gap={rowSpacingMap[labelPos][rowSpacing] + "px"}
      style:--grid-column-gap={colSpacingMap[labelPos][columnSpacing] + "px"}
      style:--grid-columns={gridColumnsDef}
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
    grid-template-columns: var(--grid-columns);
    column-gap: var(--grid-column-gap);
    row-gap: var(--grid-row-gap);
  }
  .spectrum-Form--labelsAbove {
    display: grid !important; 
  }
  .spectrum-Heading {
    font-size: var(--header-font-size) !important;
    color: var(--header-font-color) !important;
    font-weight: var(--header-font-bold) !important;
    margin-top: 0px !important;
  }

  .spectrum-Accordion-itemHeading .spectrum-Accordion-itemHeader {
    color: var(--header-font-color) !important;
    font-size: var(--header-font-size) !important;
    font-weight: var(--header-font-bold) !important;
    min-height: unset !important;
  }
</style>
