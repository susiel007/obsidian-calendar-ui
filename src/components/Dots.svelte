<script lang="ts">
  import type { IDayMetadata } from "src/types";

  export let centered: boolean = true;
  export let metadata: IDayMetadata[];
  export let isActive: boolean = false;

  let sortedMeta: IDayMetadata[];
  $: sortedMeta = metadata && metadata.sort((a, b) => a.order - b.order);
</script>

<div class="dot-container" class:centered>
  {#if metadata}
    {#each sortedMeta as { color, display, value, dots }}
      {#if display === "calendar-and-menu"}
        {#if dots && dots.length > 0}
          {#each dots as dot}
            {#if dot.isFilled}
              <svg
                class="dot filled"
                class:active="{isActive}"
                style="color:{color}"
                viewBox="0 0 6 6"
                xmlns="http://www.w3.org/2000/svg"
              >
                <circle cx="3" cy="3" r="2"></circle>
              </svg>
            {:else}
              <svg
                class="dot hollow"
                class:active="{isActive}"
                style="color:{color}"
                viewBox="0 0 6 6"
                xmlns="http://www.w3.org/2000/svg"
              >
                <circle cx="3" cy="3" r="2"></circle>
              </svg>
            {/if}
          {/each}
        {:else if value}
          <span
            class="metadata-value"
            class:active="{isActive}"
            style="color:{color}"
          >
            {value}
          </span>
        {/if}
      {/if}
    {/each}
  {/if}
</div>

<style>
  .dot-container {
    display: flex;
    flex-wrap: wrap;
    font-size: 0.6em;
    line-height: 1;
    gap: 2px;
  }

  .centered {
    justify-content: center;
  }

  .metadata-value {
    display: inline-block;
  }

  .dot {
    display: inline-block;
    height: 6px;
    width: 6px;
  }

  .filled {
    fill: currentColor;
  }

  .hollow {
    fill: none;
    stroke: currentColor;
  }

  .active {
    color: var(--text-on-accent) !important;
  }
</style>
