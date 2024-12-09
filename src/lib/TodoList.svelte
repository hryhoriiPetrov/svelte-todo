<script>
  import TodoItem from './TodoItem.svelte';

  let { todos, changeTodo, deleteTodo } = $props();
</script>

{#if Array.isArray(todos) && todos.length > 0}
  <ul>
    {#each todos as todo}
      <TodoItem {todo} {changeTodo} {deleteTodo} />
    {/each}
  </ul>
{/if}