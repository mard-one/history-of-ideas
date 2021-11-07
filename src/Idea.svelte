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
          path: fileName + '.md',
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

<SvelteMarkdown {source} />
