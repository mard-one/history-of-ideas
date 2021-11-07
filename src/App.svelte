<script>
  import { onMount } from "svelte";
  import { listOfIdeas } from "./stores";
  import { Router, Link, Route } from "svelte-routing";
  import { Octokit } from "@octokit/core";
  import ListView from "./ListView.svelte";
  import Navbar from "./Navbar.svelte";
  import TimelineView from "./TimelineView.svelte";
  import Idea from "./Idea.svelte";

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
</svelte:head>

<Router {url}>
  <Navbar>
    <Link to="ideas">List</Link>
    <Link to="timeline">Timeline</Link>
  </Navbar>
  <main>
    <Route path="ideas" component={ListView} />
    <Route path="timeline" component={TimelineView} />
    <Route path="/" component={ListView} />
  </main>
  <Route path="ideas/:fileName" let:params>
    <Idea fileName={params.fileName} />
  </Route>
</Router>

<style>
</style>
