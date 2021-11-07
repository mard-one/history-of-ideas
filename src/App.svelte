<script>
  import { onDestroy, onMount } from "svelte";
  import { listOfIdeas, timelineClass } from "./stores";
  import { Router, Route } from "svelte-routing";
  import { Octokit } from "@octokit/core";
  import ListView from "./ListView.svelte";
  import Navbar from "./Navbar.svelte";
  import TimelineView from "./TimelineView.svelte";
  import Idea from "./Idea.svelte";
  import Contribute from "./Contribute.svelte";

  onMount(async () => {
    const octokit = new Octokit();
    const ideas = [];
    try {
      const res = await octokit.request("GET /repos/{owner}/{repo}/contents", {
        owner: "mard-one",
        repo: "unit-ideas",
      });
      // console.log("res", res.data);
      for (const file of res.data) {
        // console.log("file", file);
        // console.log('file.name.includes(".json")', file.name.includes(".json"));
        if (file.name.includes(".json")) {
          // console.log("file", file);
          const data = await octokit.request(
            "GET /repos/{owner}/{repo}/contents/{path}",
            {
              headers: {
                accept: "application/vnd.github.v3.raw+json",
              },
              owner: "mard-one",
              repo: "unit-ideas",
              path: file.path,
            }
          );
          // console.log("data raw", data);
          const content = JSON.parse(data.data);
          // console.log("content", content);
          ideas.push({
            ...content,
            articleFileName: file.name.replace(".json", ""),
          });
        }
      }
    } catch (err) {
      // console.log("fetch failed");
    }
    // console.log("ideas", ideas);
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
    src="https://cdn.knightlab.com/libs/timeline3/latest/js/timeline.js"
    on:load={(e) => {
      // console.log("e", e);
      // console.log("window.TL", window.TL);

      timelineClass.set(window.TL);
    }}/>
  <div id="fb-root" />
  <script
    async
    defer
    crossorigin="anonymous"
    src="https://connect.facebook.net/ru_RU/sdk.js#xfbml=1&version=v12.0"
    nonce="F0eoCuu0"/>
</svelte:head>

<Router>
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
    padding: 0 16px;
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
  @media only screen and (max-width: 600px) {
    .gap-left {
      padding-right: 32px;
      width: 0px;
    }
    .gap-right {
      width: 0px;
      padding-left: 32px;
    }
  }
</style>
