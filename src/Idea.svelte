<script>
  import { onMount } from "svelte";
  import { Octokit } from "@octokit/core";
  import SvelteMarkdown from "svelte-markdown";

  export let fileName = "";
  let source = "";
  console.log("fileName", fileName);
  onMount(async () => {
    const octokit = new Octokit();
    try {
      const data = await octokit.request(
        "GET /repos/{owner}/{repo}/contents/{path}",
        {
          owner: "mard-one",
          repo: "unit-ideas",
          path: fileName + ".md",
        }
      );
      console.log("data", data.data.content);
      source = window.atob(data.data.content);
      console.log("content", content);
    } catch (err) {
      console.log("fetch failed");
    }
  });
</script>

<a
  href="https://github.com/mard-one/unit-ideas/blob/main/{fileName}.md"
  target="_blank"
  class="edit-article">edit this article</a
>

<SvelteMarkdown {source} />

<div
  class="fb-comments"
  data-href="https://localhost"
  data-width=""
  data-numposts="5"
/>

<style>
  .edit-article {
    position: absolute;
    right: 0;
    margin-top: 30px;
  }
</style>
