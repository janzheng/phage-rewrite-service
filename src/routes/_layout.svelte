

<script context="module">
  export async function preload(page, session) {
    console.log('Page:', page)
    return {
      page,
      path: page && page.path ? page.path.substring(1) : null
    };
  }
</script>

<script>
  export let page, path
  $: console.log('middle ware:', page, path)
</script>


<div id="top" class="ContentFrame Layout">
  {#key segment}
  
    <main class="ContentFrame-body __content-frame">
      <slot ></slot>
    </main>
    
  {/key}
  
</div>

<style global type="text/scss">
	@import '../styles/core.scss';
</style>