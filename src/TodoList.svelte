<script>
    import Todo from "./Todo.svelte";

    let lastId = 0;

    const createTodo = (text, done = false) => ({id: ++lastId, text, done});

    let todoText = '';

    let todos = [
        createTodo('Learn Svelte', true),
        createTodo('build a Svelte app')
    ];

    $: uncompleteCount = todos.filter(t => !t.done).length;
    $: status = `${uncompleteCount} of ${todos.length} remaining`;

    function addTodo() {
        todos = todos.concat(createTodo(todoText));
        todoText = ''; //clears the input
    }
    function archiveCompleted() {
        todos = todos.filter(t => !t.done);
    }
    function deleteTodo(todoId) {
        todos = todos.filter(t => t.id !== todoId);
    }
    function toggleDone(todo) {
        const {id} = todo;
        todos = todos.map(t => (t.id === id ? {...t, done: !t.done} : t));
    }
</script>

<div>
    <h1>To Do List</h1>
    <div>
        {status}
        <button on:click={archiveCompleted}>Archive Completed</button>
    </div>


<form on:submit|preventDefault>
    <input type="text" placeholder="enter new todo here" class="input w-full max-w-xs" bind:value={todoText} size="30" />
    <button disabled={!todoText} on:click={addTodo} class="add-button">Add</button>   
</form>

<ul>
    {#each todos as todo}
        <Todo
            {todo}
            on:delete={() => deleteTodo(todo.id)}
            on:toggleDone={() => toggleDone(todo)} />
    {/each}        
</ul>
</div>

<style>
    h1 {
        color: maroon;
    }
    button {
        margin-left: 10px;
    }
    ul {
        list-style: none; /* removes bullets */
        margin-left: 0;
        padding-left: 0;
    }

    input{
		outline: none;
        border: none;
        border-bottom: 1px solid lightgray;
        background-color: transparent;
	}

    button.add-button {
        border: none;
        background-color: lightgreen;
        color: white;
        padding: 10px 12px;
        cursor: pointer;
        border-radius: 5px;
        font-weight: bold;
    }

    form {
        margin-top: 20px;
    }
</style>