<script lang="ts">
  import { resolveRoute } from "$app/paths";
</script>

<ul>
  <li><a href={resolveRoute("/agent", {})}>Agent</a></li>
  <li><a href={resolveRoute("/client", {})}>Client</a></li>
</ul>
