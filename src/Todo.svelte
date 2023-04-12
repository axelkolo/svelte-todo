<script>
	import { createEventDispatcher } from 'svelte';
	import { fade } from 'svelte/transition';
	const dispatch = createEventDispatcher();

	export let todo;
</script>

<li transition:fade>
	<input
		type="checkbox"
		checked={todo.done}
		on:change={ () => dispatch('toggleDone') } />
	<span class={'done-' + todo.done}>{todo.text}</span>
	<button on:click={ () => dispatch('delete')}>Delete</button>	
</li>

<style>
	:global(body) {
		background-color: rgba(240, 231, 231, 0.205);
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}
	.done-true {
		color:red;
		text-decoration: line-through;
	}

	li {
		margin-top: 5px;
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 10px;
	}

	button {
		border: none;
		padding: 10px;
		border-radius: 6px;
		background-color: rgb(226, 54, 28);
		color: white;
		font-weight: bold;
	}

</style>