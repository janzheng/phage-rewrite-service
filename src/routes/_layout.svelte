

<script context="module">
  export async function preload(page, session) {
    const [sub, phage, pub] = page.host.split('.')
    

    // get Notion collections like Whimsy, Blog table .. — Loaded into Svelte Store for easy access
    const notionData = { }
    let index = await this.fetch(`${process.env.NOTION_API}/v1/collection/${process.env.NOTION_WHIMSY}`).then(r => r.json())

    // add all non-Controller pages into the DB
    if(index && index.rows) {
      await Promise.all(index.rows.map( async r => {
        if(r._projectType && r._projectType != 'Controller')
        notionData[r['Name']] = await this.fetch(`${process.env.NOTION_API}/v1/collection/${r['_blockid']}`).then(r => r.json())
        if(r['_path'] && r['_code']) {
          notionData[r['Name']]['_path'] = r['_path']
          notionData[r['Name']]['_code'] = r['_code']
        }
      }))
    }

    // iterate through all the projects to see if they match our page.host
    // 
    let projectDomain
    Object.values(notionData).find(project => {
      let found = project && project.rows.find(r => r['Name'] == '_domain')
      if(found) {
        projectDomain = found['_value']
        return found
      }
    })


    console.log('[Incoming]:', sub, phage, pub, '||', page.host, '>>', page, 'pDomain:', projectDomain)

    // if we're a domain, check if it matches a project, if it does redirect it
    if(page.path == projectDomain) {
      console.log('[projectDomain] sending', `https://discovery.phage.directory/${sub}${page.path}`)
      return this.redirect(301, `https://discovery.phage.directory/${sub}${page.path}`)
    }


    if(sub && phage && pub) { // dumb way to check if this subdomain exists
      console.log('[subdomain] sending', `https://discovery.phage.directory/${sub}${page.path}`)
      return this.redirect(301, `https://discovery.phage.directory/${sub}${page.path}`)
    }


    console.log(`[no match] sending to insights; ${page.path}`)
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