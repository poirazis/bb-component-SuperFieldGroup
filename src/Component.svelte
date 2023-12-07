<script>
  import { getContext, setContext } from "svelte"

  const { styleable } = getContext("sdk")
  const component = getContext("component")

  export let hideHeader
  export let header
  export let headerIcon
  export let headerFontColor = "var(--spectrum-global-color-gray-700)"

  export let labelPos = "above"
  export let labelWidth = "6rem"
  export let labelSize = "13px"
  export let labelWeight
  export let rowSpacing = "M"
  export let columnSpacing = "M"
  export let columns = 1
  export let disabled

  const rowSpacingMap = {
    "above" : { XS : 2, S: 4, M : 8, L : 16 },
    "left" : { XS : 4, S: 8, M : 12, L : 20 }
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
      "--super-field-group-span": "6",
    }
  }

  function genmerateColumns ( ) {
    return "repeat(" + columns * 6 + ", 1fr )";
  }

  $: setContext("field-group", labelPos )
  $: setContext("field-group-columns", columns )
  $: setContext("field-group-label-width", labelWidth )
  $: setContext("field-group-disabled", disabled )
</script>


<div class="super-field-group" use:styleable={$component.styles} >
  {#if !hideHeader}
    <div class="field-group-header" style:height={labelPos=="above" ? "2rem" : "2.5rem"}>
      {#if headerIcon}
        <i class={headerIcon} color={headerFontColor} style:font-size={"16px"}/>
      {/if}
      <h4 style:margin={"unset"} style:color={headerFontColor || "var(--spectrum-global-color-gray-700)"} >
        {header?.toUpperCase()}
      </h4>
    </div>
  {/if}

  <div
    class="spectrum-Form"
    class:spectrum-Form--labelsAbove={labelPos === "above"}
    style:--grid-row-gap={rowSpacingMap[labelPos][rowSpacing] + "px"}
    style:--grid-column-gap={colSpacingMap[labelPos][columnSpacing] + "px"}
    style:--grid-columns={gridColumnsDef}
    style:--label-placement={labelPos == "left" ? "row" : "column"}
    style:--label-column-width={labelWidth || "fit-contents"}
    style:--label-gap={labelPos === "above" ? "0rem" : "0.85rem"} 
    style:--spectrum-tableform-margin={"unset"}
    style:--spectrum-tableform-border-spacing={"unset"}
    style:--label-font-size={labelSize}
    style:--label-font-weight={labelWeight}
  >   
    {#key labelPos + labelWidth}
      <slot />
    {/key}
  </div>

</div>

<style>
  .super-field-group {
    position: relative;
    display: flex;
    flex-direction: column;
    gap: 0rem;
  }

  .field-group-header {
    display: flex;
    align-items: flex-start;
    margin: unset;
    gap: 0.5rem
  }
  .spectrum-Form {
    width: 100%;
    display: grid;
    grid-template-columns: var(--grid-columns);
    column-gap: var(--grid-column-gap);
    row-gap: var(--grid-row-gap);
    align-items: flex-start;
    justify-items: stretch;
  }
  .spectrum-Form--labelsAbove {
    display: grid !important; 
    align-items: stretch;
    justify-items: stretch;
  }

  :global(.spectrum-Form > .component > * ) {
    grid-column: span var(--super-field-group-span);
  }

 :global(.super-field-group > .spectrum-Form > .component > .spectrum-Form-item.above ) {
    line-height: 20px !important;
    flex-direction: var(--label-placement) !important;
    gap: 0.85rem;
  }
 :global(.super-field-group > .spectrum-Form > .component > .spectrum-Form-item > .spectrum-Form-itemLabel ) {
    min-width: var(--label-column-width) !important;
    font-size: var(--label-font-size) !important;
    font-weight: var(--label-font-weight) !important;
  }
</style>
