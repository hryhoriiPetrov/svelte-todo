<script>
  let { value = $bindable('todoFormTitle'), createTodo } = $props();
</script>

<form onsubmit={(e) => createTodo(e)}>
  <input
    type="text"
    placeholder="Введите задачу..."
    bind:value={value}
  />
</form>