
<script>
	import { ProjectData, _getPost } from "@/stores/whimsy"
  import Notion from '@yawnxyz/svelte-notion'

</script>


<svelte:head>
	<!-- <title>{article.title}</title> -->
</svelte:head>


{#if $ProjectData && $ProjectData.blog && $ProjectData.blog.rows}

  <div class='_section-page _margin-center _divider-top _divider-bottom'>
    <h2>Posts</h2>
    {#each $ProjectData.blog.rows as post}
      {#if process.env.STATUS == 'Published'}
        {#if post.Status == 'Published'}
          <div class="Post">
            {#if post.Slug}
              <a href="/blog/{post.Slug}"><h3>{post.Name}</h3></a>
            {:else}
              <h3 class="_planned">{post.Name} <span class="_font-normal">(planned)</span></h3>
            {/if}
          </div>
        {/if}
      {:else}
        <div class="Post">
          {#if post.Slug}
            <a href="/blog/{post.Slug}"><h3 class="_padding-bottom">{post.Name}</h3></a>
          {:else}
            <h3 class="_planned _padding-bottom">{post.Name} <span class="_font-normal">(planned)</span></h3>
          {/if}
          <!-- <span class="_tag">{post.Status}</span> -->
        </div>
      {/if}
    {/each}
  </div>

{/if}



<style>
  h3._planned {
    color: #666;
  }
</style>