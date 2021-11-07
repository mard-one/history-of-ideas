<script>
  import { onMount } from "svelte";

  import { listOfIdeas } from "./stores.js";
  console.log("$listOfIdeas", $listOfIdeas);

  const init = () => {
    listOfIdeas.subscribe((ideas) => {
      console.log("subscribe ideas", ideas);
      if (window.TL && ideas) {
        new window.TL.Timeline("timeline-embed", {
          scale: "human",
          title: {
            media: {
              caption: "",
              credit: "",
              url: "",
              thumbnail: "",
            },
            text: {
              headline: "The Republican Run-Up",
              text: "Follow the presidential caucuses and primaries as members of the Republican Party race to become the GOP's next presidential nominee.",
            },
          },
          events: ideas,
        });
      }
    });
  };
  onMount(init);
</script>

<div id="timeline-embed" style="height: 800px" />
