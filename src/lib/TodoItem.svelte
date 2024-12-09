<script>
  const { todo, changeTodo, deleteTodo } = $props();
</script>

<li>
  <label>
    <span>{todo.title}</span>
    <input
      type="checkbox"
      checked={todo.completed}
      onchange={() => changeTodo(todo)}
    />
  </label>
  <button
    type="button"
    aria-label="Удалить задачу"
    onclick={() => deleteTodo(todo.id)}
  >
    x
  </button>
</li>