<script>
  import { listOfIdeas, timelineClass } from "./stores.js";
  let timeline, ideas;

  timelineClass.subscribe((tl) => {
    // console.log("subscribe tl", tl);
    timeline = tl;
  });
  listOfIdeas.subscribe((i) => {
    // console.log("subscribe ideas", ideas);
    ideas = i;
  });
  // console.log("timeline", timeline);
  // console.log("ideas", ideas);
  $: if (timeline && ideas.length) {
    // console.log("timeline init");
    let timelineRef = new timeline.Timeline(
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
        font: 'bitter-raleway'
        // font: 'georgia-helvetica'
        // font: 'lustria-lato'
        // font: 'opensans-gentiumbook'
        // font: 'rufina-sintony'
      }
    );
    // console.log('timeline', timeline);
    timelineRef.on("ready", (e) => {
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
</script>

<div
  id="timeline-embed"
  style="height: calc(100vh - 70px); overflow: visible"
/>
