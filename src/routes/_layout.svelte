

<script context="module">
  export async function preload(page, session) {
    const [sub, phage, pub] = page.host.split('.')

    console.log('sub:', sub, phage, pub, '||', page.host, '>>', page)
    if(sub && phage && pub) {// dumb way to check if this exists
      console.log('sending', `https://discovery.phage.directory/${sub}${page.path}`)
      return this.redirect(301, `https://discovery.phage.directory/${sub}${page.path}`)
    }

    return this.redirect(301, `https://phage.directory/insights`)
  }
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