<script>
	import { createEventDispatcher } from 'svelte';
	
    export let type; 
	export let flipped;
    export let invis;
	
	const dispatch = createEventDispatcher();
</script>

<div class="card-outer" on:click={() => dispatch('click')}>
    <div class="card-inner" class:flipped>
        <div class="card-front center" class:invis>
            ?
        </div>

        <div class="card-back center">
            {type}
        </div>
    </div>
</div>

<style>
    .center {
        display: flex;
        justify-content: center;
        align-items: center;
        text-align: center;
    }

    .card-outer {
        perspective: 1000px;
        background-color: transparent;
        width: 8rem; height: 10rem;
        margin-right: 0.5em; margin-bottom: 0.5em;
    }

    .card-inner {
        position: relative;
        width: 100%; height: 100%;

        font-size: 3em;
        font-weight: 600;
        color: white;

        transition: transform 0.8s;
        transform-style: preserve-3d;
    }

    .flipped {
        transform: rotateY(180deg);
    }

    .card-front, .card-back {
        width: 100%; height: 100%;
        border-radius: 0.25em;
        box-shadow: 2px 2px 5px 0 rgba(19, 19, 19, 0.5);
        position: absolute;
        backface-visibility: hidden;
    }

    .card-front {
        background: linear-gradient(#2e2e2e, #272727);
    }

    .invis {
        opacity: 0;
    }

    .card-back {
        transform: rotateY(180deg);
        background: linear-gradient(#4c3aee, #3624df);
    }
</style>