<script lang="ts">
  import type { Peer } from "peerjs";
  let client_code = $state<string>();
  let agent_code = $state<string>();
  let loading = $state(false);

  let peer: Peer;
  async function startPeering() {
    const { Peer } = await import("peerjs");
    peer = new Peer();
    peer.on("open", (id) => {
      client_code = id;
    });
  }

  async function call() {
    if (!agent_code) return;
    loading = true;
    const cc = peer.connect(agent_code);
    cc.on("open", async () => {
      loading = false;
      const stream =
        await navigator.mediaDevices.getDisplayMedia(displayMediaOptions);
      peer.call(agent_code!!, stream);
    });
    cc.on("error", () => {
      console.log("error");
      loading = false;
    });
  }

  $effect(() => {
    startPeering();
  });

  const displayMediaOptions: DisplayMediaStreamOptions = {
    video: {
      displaySurface: "monitor",
    },
    audio: false,
  };
</script>

<div>
  <input type="text" required bind:value={agent_code} />
  <button type="button" disabled={loading} on:click={call}>Call</button>
</div>
{#if loading}
  <progress aria-busy="true"></progress>
{/if}
