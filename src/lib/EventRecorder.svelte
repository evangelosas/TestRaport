<script>
import { createEventDispatcher } from "svelte";
const dispatch = createEventDispatcher();
export let url;
const socket = new WebSocket(url);
const START_PLAYING = true;

socket.addEventListener('message', function (event) {
  let value = event.data;
    console.log('Message from server: '+ value +" on " + new Date().toString() );
    dispatch("updateAnimation", {
      index: convertValueToIndex(value),
      status: START_PLAYING,
    });
});

function convertValueToIndex(value) {
  return Math.floor(value * 3) + 1;
}

</script>
