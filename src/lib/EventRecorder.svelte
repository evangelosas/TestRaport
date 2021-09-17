<script>
  import * as socketIO from "socket.io";

  var socket = io();
  socket.on("animation", function (msg) {
    console.log(msg);
  });
</script>
