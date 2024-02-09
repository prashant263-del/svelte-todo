<svelte:options immutable={true} />
<script>
	import {createEventDispatcher} from "svelte";
	export let todos = [];
	let newTodoTxt = '';
	const dispatch = createEventDispatcher();
	
	function handleAddTodo(){
		const isNotCancelled = dispatch ('addtodo',
								{
									title: newTodoTxt
								},{cancelable: true});
		if(isNotCancelled){
			newTodoTxt = '';
		}
	}
function handleRemoveTodo(id){
	dispatch('removeTodo',
					 {
						 id
					 }
					)
}

		function handleToggleTodo(id, value){
		dispatch('toggletodo',
						 {
							 id, value
						 })
	}
</script>

<div>
	<form on:submit|preventDefault={handleAddTodo}>
		<input bind:value={newTodoTxt}/>
		<button type="submit">Add</button>
	</form>
	<ul>
		{#each todos as {id, title, completed } (id)}
			<!-- {@const number = index+1} -->
			<li>
				<label>
					<input on:input={(event) => {event.currentTarget.checked = completed; handleToggleTodo(id, !completed); }} type="checkbox" checked={completed}/>{title}</label>
			<button on:click={()=>handleRemoveTodo(id)}>Remove</button>
			</li>
		{/each}
	</ul>
</div>