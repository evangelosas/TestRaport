<script>
import { createEventDispatcher } from "svelte";
import { numberOfAnimations } from "./AnimationStore.js";

const dispatch = createEventDispatcher();
export let url;
const socket = new WebSocket(url);
const START_PLAYING = true;

socket.addEventListener('message', function (event) {
  let value = event.data;
  console.log('Message from server: '+ value +" on " + new Date().toString() );
  if ($numberOfAnimations > 0) {
    dispatch("updateAnimation", {
      index: convertValueToIndex(value),
      status: START_PLAYING,
    });
  }
});

function convertValueToIndex(value) {
  return Math.floor(value * $numberOfAnimations) + 1;
}

</script>
