<script>
    import { onMount } from 'svelte';
    import { browser } from '$app/environment';

    let todoText = '';
    let todos = [];

    onMount(() => {
        if (browser) {
            const storedTodos = JSON.parse(localStorage.getItem('todos'));
            if (storedTodos) {
                todos = storedTodos;
            }
        }
    });

    function saveTodos() {
        // Save todos to local storage
        if (browser) {
            localStorage.setItem('todos', JSON.stringify(todos));
        }
    }

    function addTodo() {
        const timestamp = new Date().toLocaleString(); // Get current timestamp
        todos.push({ text: todoText, done: false, timestamp });
        todos = todos;
        todoText = '';
        // Save todos to local storage
        saveTodos();
    }

    function remove(index) {
        // Delete entry
        todos.splice(index, 1);
        todos = todos;
        saveTodos();
    }
</script>

<h1>TODO APP</h1>

<input type="text" class="todo-input" bind:value={todoText} />
<button on:click={addTodo}>ADD</button>

{#each todos as todo, index}
    <div class="todo-entry" class:done={todo.done}>
        <div>{todo.text}</div>
        <div class="timestamp">Added: {todo.timestamp}</div> <!-- Display timestamp -->
        <input type="checkbox" bind:checked={todo.done} />
        <button
            class="delete"
            on:click={() => {
                remove(index);
            }}
        >
            X
        </button>
    </div>
{/each}

<style>
    *{
        font-family: Arial, Helvetica, sans-serif;
    }
    .delete {
            background-color: white;
            border: none;
    }
    .delete:hover {
            background-color: red;
            font-weight: 700;
    }
    .done {
            color: green;
    }
    .todo-entry {
            display: flex;
    }
    .timestamp {
        font-size: 0.8em;
        color: gray;
    }
</style>
