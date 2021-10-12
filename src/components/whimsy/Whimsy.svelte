

<script>
  import Notion from '@yawnxyz/svelte-notion'
  // import Notion from '../../../../../../svelte-notion/src/Notion.svelte'

	import { headConstructor } from '@/_project/head.js';
	import { _get, _find, Project } from "@/stores/whimsy"

  import { stores } from '@sapper/app'
  const { page } = stores()


  export let project, path, slug = '/'

  // should be a page endpoint
  // support collections in the future
  let projPage = _get(slug)
  let _css = _find('_css')
  let _title = _find('_title')
  let _head = _find('_head') // this overrides the other stuff
  let _icon = _find('_icon') && _find('_icon')['Files'] && _find('_icon')['Files'][0] ? _find('_icon')['Files'][0]['url'] : undefined
  let _shareImg = _find('_share-img') && _find('_share-img')['Files'] && _find('_share-img')['Files'][0] ? _find('_share-img')['Files'][0]['url'] : undefined


  let head = headConstructor({
    site_title: _find('_title')['_value'], 
    site_url: `https://${$page.host}`,
    // description:,
    site_ico: _icon,
    site_image: _shareImg,
    // author, 
    // twitterCreator, // author's twitter
    // twitterCard,
    // meta,
  })


  // let dummy = `
  //   h1 { 
  //     color: red !important;
  //   }

  // `

</script>


<svelte:head>
	{#if head}
		<title>{ head.title }</title>
		{#if head.link}
			{#each head.meta as meta}
				<meta 
					charset={meta.charset}
					data-hid={meta.hid} 
					name={meta.name} 
					content={meta.content} 
					property={meta.property} 
				>
			{/each}
			{#each head.link as link}
				<link data-hid={link.hid} rel={link.rel} href={link.href}>
			{/each}
		{/if}
	{/if}
  {#if _head && _head._value}{
    @html `${_head._value}`}
  {/if}
</svelte:head>




{#if project}
  <div class='_section-page _section-notion _margin-center _margin-top _divider-bottom'>
    <!-- <h1>{slug}</h1> -->
    <!-- <h2>{project.name}</h2> -->
    {#if projPage && projPage.id}
      <Notion loadingMsg='' classes={''} id={projPage.id} api={process.env.NOTION_API} />
    {/if}
    
  </div>

{:else}
  <div class='_section-page _margin-center _divider-top _divider-bottom'>
    <h1>No project found</h1>
  </div>
{/if}


<!-- inject styles -->
{#if _css && _css._value}
  {@html `<style>${_css._value}</style>`}
{/if}