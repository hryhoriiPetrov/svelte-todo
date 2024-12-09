<script>
  import { onMount } from 'svelte';
  import TodoForm from './lib/TodoForm.svelte';
  import TodoList from './lib/TodoList.svelte';

  let todos = $state([]);
  let todoFormTitle = $state('');

  async function createTodo(e) {
    e.preventDefault();

    const todoObj = {
      title: todoFormTitle,
      completed: false,
    }

    try {
      const response = await fetch('https://c6fa07d88d475c61.mokky.dev/items', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(todoObj),
      });

      if (!response.ok) {
        throw new Error(`Статус ошибки: ${response.status}`);
      }

      const newTodo = await response.json();

      todos.push(newTodo);
      todoFormTitle = '';
    } catch (error) {
      console.error(error.message);
    }
  }

  async function getAllTodos() {
    try {
      const response = await fetch('https://c6fa07d88d475c61.mokky.dev/items');

      if (!response.ok) {
        throw new Error(`Статус ошибки: ${response.status}`);
      }

      todos = await response.json();
    } catch (error) {
      console.error(error.message);
    }
  }

  async function deleteTodo(id) {
    try {
      const response = await fetch(`https://c6fa07d88d475c61.mokky.dev/items/${id}`, {
        method: 'DELETE',
        headers: {
          'Content-Type': 'application/json',
        },
      });

      if (!response.ok) {
        throw new Error(`Статус ошибки: ${response.status}`);
      }

      todos = todos.filter((todo) => todo.id !== id);
    } catch (error) {
      console.error(error.message);
    }
  }

  async function changeTodo(todo) {
    try {
      const response = await fetch(`https://c6fa07d88d475c61.mokky.dev/items/${todo.id}`, {
        method: 'PATCH',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ completed: !todo.completed})
      });

      if (!response.ok) {
        throw new Error(`Статус ошибки: ${response.status}`);
      }

      todos = todos.map((el) => {
        if (el.id === todo.id) {
          return {
            ...el,
            completed: !todo.completed,
          }
        }

        return el;
      });
    } catch (error) {
      console.error(error.message);
    }
  }

  onMount(async () => {
    await getAllTodos();
  });
</script>

<main>
  <h1>Список задач</h1>
  <TodoForm bind:value={todoFormTitle} {createTodo} />
  <TodoList {todos} {changeTodo} {deleteTodo} />
</main>