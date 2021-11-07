<script>
  import { onMount } from "svelte";
  import { Octokit } from "@octokit/core";
  // import SvelteMarkdown from "svelte-markdown";

  export let fileName = "";
  let source = null;
  // console.log("fileName", fileName);
  onMount(async () => {
    const octokit = new Octokit();
    try {
      const data = await octokit.request(
        "GET /repos/{owner}/{repo}/contents/{path}",
        {
          headers: {
            accept: "application/vnd.github.v3.html+json",
          },
          owner: "mard-one",
          repo: "unit-ideas",
          path: fileName + ".md",
        }
      );
      source = data.data;
      // console.log("source", source);
      if (window.FB) {
        FB.XFBML.parse();
      }
    } catch (err) {
      // console.log("fetch failed");
    }
  });
</script>

<a
  href="https://github.com/mard-one/unit-ideas/blob/main/{fileName}.md"
  target="_blank"
  class="edit-article">edit this article</a
>

<!-- <SvelteMarkdown {source} /> -->
{@html source}
<br />
<br />
<br />
<div
  class="fb-comments"
  data-href="https://localhost"
  data-width="100%"
  data-numposts="5"
/>

<style>
  .edit-article {
    position: absolute;
    right: 0;
    margin-top: 30px;
  }
</style>
