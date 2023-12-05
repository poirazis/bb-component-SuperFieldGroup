<script>
  import { getContext, setContext } from "svelte"

  const { styleable, builderStore, componentStore } = getContext("sdk")
  const component = getContext("component")
  const superContainer = getContext("superContainer")
  const superContainerParams = getContext("superContainerParams")

  export let hideHeader
  export let header
  export let headerSize
  export let headerFontBold
  export let headerFontColor = "var(--spectrum-global-color-gray-700)"
  export let labelPos = "above"
  export let labelWidth = "10rem"
  export let labelSize = "14px"
  export let labelWeight
  export let rowSpacing = "M"
  export let columnSpacing = "M"
  export let collapsible
  export let collapsed = false
  export let padding = true
  export let columns = 1
  export let disabled

  // Super Options
  export let inSuperContainer
  export let colSpan = 1
  export let rowSpan = 1

  const rowSpacingMap = {
    "above" : { XS : 2, S: 4, M : 8, L : 16 },
    "left" : { XS : 4, S: 8, M : 16, L : 24 }
  }

  const colSpacingMap = {
    "above" : { XS : 2, S: 8, M : 16, L : 24 },
    "left" : { XS : 2, S: 8, M : 16, L : 24 },
  }

  $: gridColumnsDef = genmerateColumns( columns )
  $: $component.styles = {
    ...$component.styles,
    normal : {
      ...$component.styles.normal,
      "grid-column" : $superContainer == "grid" ? "span " + colSpan : "span " + ( colSpan * 6 ),
      "grid-row" : $superContainer == "grid" ? "span " + rowSpan : "span 1"
    }
  }

  $: {
    if (
      $builderStore.inBuilder
      && $componentStore.selectedComponentPath?.includes($component.id)
      && $superContainer == "grid" && !inSuperContainer
    ) {
      builderStore.actions.updateProp("inSuperContainer", true)
    } else if (
      $builderStore.inBuilder
      && $componentStore.selectedComponentPath?.includes($component.id)
      && inSuperContainer && $superContainer != "grid"
    ) {
      builderStore.actions.updateProp("inSuperContainer", false)
    }
  }

  function genmerateColumns ( ) {
    return "repeat(" + columns * 6 + ", 1fr )";
  }

  const superMeUp = (node, sc ) => {
    console.log( sc )  
  }

  $: setContext("field-group", labelPos )
  $: setContext("field-group-columns", columns )
  $: setContext("field-group-label-width", labelWidth )
</script>


<div class="wrapper" use:styleable={$component.styles} use:superMeUp={$superContainer} >
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
            {#key labelPos}
              <slot />
            {/key}
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
        style:margin-bottom={"1.25rem"}
        >
        {header?.toUpperCase()}
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
      style:--super-field-group-span={6}
    >   
      {#key labelPos}
        <slot />
      {/key}
    </div>
    {/if}
  </div>

<style>
  .wrapper {
    width: 100%;
    position: relative;
  }

  :global(.wrapper > .spectrum-Form > .component > *) {
    grid-column: span var(--super-field-group-span);
  }

 :global(.spectrum-Form-itemLabel) {
    min-width: var(--label-column-width) !important;
    font-size: var(--label-font-size) !important;
    font-weight: var(--label-font-weight) !important;
    padding-right: 0.85rem;
    line-height: 24px !important;
  }

  .spectrum-Form {
    width: 100%;
    display: grid;
    grid-template-columns: var(--grid-columns);
    column-gap: var(--grid-column-gap);
    row-gap: var(--grid-row-gap);
    align-items: stretch;
    justify-items: stretch;
  }
  .spectrum-Form--labelsAbove {
    display: grid !important; 
    align-items: stretch;
    justify-items: stretch;
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
