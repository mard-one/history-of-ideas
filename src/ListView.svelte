<script>
  import { listOfIdeas } from "./stores.js";
  import { Link } from "svelte-routing";
</script>

<h1>List view</h1>

<ul>
  {#each $listOfIdeas as idea}
    <li>
      <Link to="ideas/{idea.articleFileName}">{idea.text.headline}</Link>
    </li>
  {/each}
</ul>
