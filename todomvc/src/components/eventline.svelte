<script>
  export let y1, y2, x1, x2;
</script>

<g class="tooltip-lines" transform="translate(0, 0)">
  <!-- horizontal line -->
  <line {y1} {y2} {x1} {x2} />
</g>

<style>
  .tooltip-lines line {
    stroke: #939393;
    stroke-dasharray: 3;
  }
</style>
