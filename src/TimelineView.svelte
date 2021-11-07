<script>
  import { onMount } from "svelte";

  import { listOfIdeas } from "./stores.js";
  // console.log("$listOfIdeas", $listOfIdeas);

  const init = () => {
    listOfIdeas.subscribe((ideas) => {
      // console.log("subscribe ideas", ideas);
      if (window.TL && ideas) {
        let timeline = new window.TL.Timeline(
          "timeline-embed",
          {
            scale: "cosmological",
            title: {
              media: {
                caption: "",
                credit: "",
                url: "",
                thumbnail: "",
              },
              text: {
                headline: "History of ideas",
                text: "The history of ideas that is a collection of everything that humankind has thought, invented, created, considered, and perfected from the beginning of civilization to these days",
              },
            },
            events: ideas,
          },
          {
            optimal_tick_width: 50,
            hash_bookmark: true,
            duration: 500,
            debug: true,
          }
        );
        // console.log('timeline', timeline);
        timeline.on("ready", (e) => {
          // console.log("now ready");
          for (const idea of e.target.config.events) {
            const container = document.getElementById(idea.unique_id);
            // console.log("container", container);
            const link = document.createElement("a");
            link.innerText = "Learn more...";
            link.setAttribute("target", "_self");
            link.setAttribute("href", `/ideas/${idea.articleFileName}`);
            container.querySelector(".tl-text-content").appendChild(link);
          }
        });
      }
    });
  };
  onMount(init);
</script>

<div id="timeline-embed" style="height: calc(100vh - 70px); overflow: visible" />
