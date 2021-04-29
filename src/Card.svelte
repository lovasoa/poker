<script lang="ts">
    import {COLOR_NAMES, RANK_NAMES} from "./_constants";
    export let color : string = "1";
    export let rank : string = "spade";
    export let hidden = false;
    $: card_name = hidden ? 'back' : `${color}_${rank}`;
</script>
<svg viewBox="0 0 169.075 244.64">
   <use xlink:href="svg-cards.svg#{card_name}"></use>
</svg>