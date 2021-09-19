<script lang="ts">
  import { createEventDispatcher } from "svelte";
  import { numberOfAnimations } from "./AnimationStore.js";

  const dispatch = createEventDispatcher();
  export let url: string;
  const socket = new WebSocket(url);
  const START_PLAYING: boolean = true;

  socket.addEventListener("message", function (event) {
    let value = event.data;
    console.log(
      "Message from server: " + value + " on " + new Date().toString()
    );
    if ($numberOfAnimations > 0) {
      dispatch("updateAnimation", {
        index: convertValueToIndex(value),
        status: START_PLAYING,
      });
    }
  });

  function convertValueToIndex(value: number): number {
    return Math.floor(value * $numberOfAnimations) + 1;
  }
</script>
