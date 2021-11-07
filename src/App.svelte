<script>
  import { onMount } from "svelte";
  import { listOfIdeas } from "./stores";
  import { Router, Route } from "svelte-routing";
  import { Octokit } from "@octokit/core";
  import ListView from "./ListView.svelte";
  import Navbar from "./Navbar.svelte";
  import TimelineView from "./TimelineView.svelte";
  import Idea from "./Idea.svelte";
  import Contribute from "./Contribute.svelte";

  export let url = "";

  onMount(async () => {
    // const res = await fetch(`https://jsonplaceholder.typicode.com/photos?_limit=20`);
    // photos = await res.json();
    console.log("start");
    const octokit = new Octokit();
    const ideas = [];
    try {
      const res = await octokit.request("GET /repos/{owner}/{repo}/contents", {
        owner: "mard-one",
        repo: "unit-ideas",
      });
      console.log("res", res.data);
      for (const file of res.data) {
        console.log("file", file);
        console.log('file.name.includes(".json")', file.name.includes(".json"));
        if (file.name.includes(".json")) {
          console.log("file", file);
          const data = await octokit.request(
            "GET /repos/{owner}/{repo}/contents/{path}",
            {
              owner: "mard-one",
              repo: "unit-ideas",
              path: file.path,
            }
          );
          // console.log("data", data);
          const content = JSON.parse(window.atob(data.data.content));
          console.log("content", content);
          ideas.push({
            ...content,
            articleFileName: file.name.replace(".json", ""),
          });
        }
      }
    } catch (err) {
      console.log("fetch failed");
    }
    console.log("ideas", ideas);
    listOfIdeas.set(ideas);
  });
</script>

<svelte:head>
  <link
    title="timeline-styles"
    rel="stylesheet"
    href="https://cdn.knightlab.com/libs/timeline3/latest/css/timeline.css"
  />
  <script
    src="https://cdn.knightlab.com/libs/timeline3/latest/js/timeline.js"></script>
  <div id="fb-root" />
  <script
    async
    defer
    crossorigin="anonymous"
    src="https://connect.facebook.net/ru_RU/sdk.js#xfbml=1&version=v12.0"
    nonce="F0eoCuu0"></script>
</svelte:head>

<Router {url}>
  <Navbar />
  <main>
    <Route path="timeline"><TimelineView /></Route>
    <div class="container">
      <div class="gap gap-left" />
      <div class="content">
        <Route path="ideas"><ListView /></Route>
        <Route path="ideas/:fileName" let:params>
          <Idea fileName={params.fileName} />
        </Route>
        <Route path="contribute"><Contribute /></Route>
      </div>
      <div class="gap gap-right" />
    </div>
    <Route path="/"><TimelineView /></Route>
  </main>
</Router>

<style>
  .container {
    display: flex;
    justify-content: space-between;
  }

  .gap-left {
    padding-right: 32px;
    width: 200px;
  }
  .content {
    position: relative;
    flex-grow: 1;
    max-width: 1080px;
  }
  .gap-right {
    width: 100px;
    padding-left: 32px;
  }
</style>
