<script lang="ts">
  import classNames from 'classnames';
  import type { LinkType } from '../types';
  import Popper from '../utils/Popper.svelte';

  export let items: LinkTypeLike[] = [];
  export let full: boolean = false;
  export let open: boolean = false;

  interface LinkTypeLike extends LinkType {
    [propName: string]: any;
  }

  let wrapperClass: string;
  $: wrapperClass = classNames(full && 'border-y w-full', $$props.class);

  let ulClass: string;
  $: ulClass = classNames(
    'grid grid-flow-row gap-y-4 md:gap-x-0 auto-col-max auto-row-max',
    full && $$slots.extra ? 'grid-cols-2' : 'grid-cols-2 md:grid-cols-3',
    'text-sm font-medium',
    full && $$slots.extra && 'md:w-2/3'
  );
</script>

<Popper
  color={full ? 'default' : 'dropdown'}
  border={!full}
  rounded={!full}
  activeContent
  arrow={false}
  trigger="click"
  placement="bottom"
  yOnly={full}
  {...$$restProps}
  class={wrapperClass}
  on:show
  bind:open>
      {#each items as item, index}
        <li>
          <slot {item} {index} />
        </li>
      {:else}
        <slot />
      {/each}
</Popper>
