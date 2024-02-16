<script lang="ts">
  let agent_code = $state<string>();
  let videoElement: HTMLVideoElement;

  async function connect() {
    const { Peer } = await import("peerjs");
    const peer = new Peer();
    peer.on("open", (id) => {
      agent_code = id;
    });

    peer.on("connection", (c) => {
      c.on("data", (data) => {
        console.log({ data });
      });
    });

    peer.on("call", (connection) => {
      console.log({ call: connection });
      connection.answer(null as any);
      connection.on("stream", (stream) => {
        videoElement.srcObject = stream;
        videoElement.play();
      });
      connection.on("close", () => {
        videoElement.srcObject = null;
      });
    });
  }

  $effect(() => {
    connect();
  });
</script>

{#if agent_code}
  <h1>{agent_code}</h1>
  <div>
    <video autoplay bind:this={videoElement}></video>
  </div>
{/if}
