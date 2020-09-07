<script>
  import {
    Button,
    Input,
    Card,
    CardBody,
    CardFooter,
    CardHeader,
    CardSubtitle,
    CardText,
    CardTitle
  } from 'sveltestrap';


  const addTask = (title, listId) => {
    const newId = (1 + Object.keys(tasks).length).toString();
    tasks[newId] = { title };
    lists[listId].tasks = [...lists[listId].tasks, Object.keys(tasks).length.toString()];
  };

  const addCurrentTask = task => {
    currentTasks = [...currentTasks, task];
    currentBoard = currentTasks.length ? tasks[currentTasks[currentTasks.length - 1]] : mb;
  };

  const removeCurrentTask = task => {
    while (currentTasks[currentTasks.length - 1].id !== task.id) {
      currentTasks.pop();
    }
    currentTasks.pop();
    currentTasks = currentTasks;
    currentBoard = currentTasks.length ? tasks[currentTasks[currentTasks.length - 1]] : mb;
  };

  const breakTask = taskId => {
    const getId = () => Math.random() * 1000000 + 10 | 0;
    const newLists = [
      { [getId()]: { title: 'To Do', tasks: []}},
      { [getId()]: { title: 'Doing', tasks: []}},
      { [getId()]: { title: 'Done', tasks: []}},
    ];
    newLists.forEach(l => {
      const lid = Object.keys(l)[0];
      lists[lid] = l[lid];
    });
    tasks[taskId].lists = newLists.map(l => Object.keys(l)[0]);
    addCurrentTask(taskId);
  };

  let currentTasks = [];
  let newTaskNames = [];
  let mb = {
    title: 'Board',
    lists: ['1', '2', '3'],
  };

  let lists = {
    '1': {
      title: 'To Do',
      tasks: ['1', '2'],
    },
    '2': {
      title: 'Doing',
      tasks: ['3', '5'],
    },
    '3': {
      title: 'Done',
      tasks: ['4', '6'],
    },
    '4': {
      title: 'To Do',
      tasks: ['7'],
    },
    '5': {
      title: 'Doing',
      tasks: ['8'],
    },
    '6': {
      title: 'Done',
      tasks: ['9'],
    },
  };

  let tasks = {
    '1': {
      title: 'Task 1',
      lists: ['4', '5', '6'],
    },
    '2': {
      title: 'Task 2',
    },
    '3': {
      title: 'Task 3',
    },
    '4': {
      title: 'Task 4',
    },
    '5': {
      title: 'Task 5',
    },
    '6': {
      title: 'Task 6',
    },
    '7': {
      title: 'Task 7',
    },
    '8': {
      title: 'Task 8',
    },
    '9': {
      title: 'Task 9',
    },
  };
  let currentBoard = currentTasks.length ? tasks[currentTasks[currentTasks.length - 1]] : mb;
  
</script>

<main>
  <div id="main">
    <div class="container">
      <h1>Main board</h1>
      {#each currentTasks as currentTaskId}
        <div class="current-task">
          <Button
            outline color="primary" size="sm"
            on:click={() => removeCurrentTask(currentTaskId)}
          >
            Up
          </Button>
          <span class="current-task-title">{tasks[currentTaskId].title}</span>
        </div>
      {/each}
      <div class="lists">
        {#each currentBoard.lists as listId}
          <div class="list">
            <h2 class="list-title">{lists[listId].title}</h2>
            <ul class="list-tasks">
              {#each lists[listId].tasks as taskId}
                <li class="task">
                  <Card>
                    <CardHeader>
                      <span class="small-card-title">
                        {taskId}: {tasks[taskId].title}
                      </span>
                    </CardHeader>
                    <CardBody>
                      {#if tasks[taskId].lists}
                        <Button outline color="primary" size="sm"
                          on:click={() => addCurrentTask(taskId)}
                        >
                          Expand
                        </Button>
                      {:else}
                        <Button outline color="primary" size="sm"
                          on:click={() => breakTask(taskId)}
                        >
                          Break
                        </Button>
                      {/if}
                    </CardBody>
                  </Card>
                </li>
              {/each}
            </ul>
            <div class="list-actions">
              <div class="new-task-input">
                <Input
                  bind:value={newTaskNames[listId]}
                  placeholder="Task title"
                />
              </div>
              <Button
                outline color="primary" size="sm"
                on:click={() => addTask(newTaskNames[listId], listId)}
              >
                Add task
              </Button>
            </div>
          </div>
        {/each}
      </div>
    </div>
  </div>
</main>

<style>
  @media (min-width: 640px) {
    .container {
      display: flex;
      flex-direction: column;
    }

    .current-task {
      margin: 10px 0;
    }

    .lists {
      display: flex;
      flex-direction: row;
    }

    .list {
      border: 1px solid black;
      width: 250px;
    }
    .list + .list {
      margin-left: 20px;
    }

    .list-title {
      margin: 10px;
      font-size: 1.1em;
    }

    .list-tasks {
      list-style-type: none;
      margin-left: 0;
      padding-left: 0;
      margin-bottom: 0;
    }

    .list-actions {
      display: flex;
      margin: 10px;
      align-items: flex-end;
      flex-direction: column;
    }

    .new-task-input {
      margin-bottom: 10px;
    }
  
    .task {
      margin: 10px;
    }

    .task:last-child {
      margin-bottom: 0;
    }

    .small-card-title {
      margin-bottom: 0;
    }

    .new-task-input {
      width: 100%;
    }
  }
</style>
