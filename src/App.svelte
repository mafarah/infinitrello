<script>
  let mb = {
    id: 1,
    title: 'Task 1',
    lists: {
      list1: [{
        id: 1,
        title: 'Task 1',
        lists: {
          list5: [{
            id: 5,
            title: 'Task 5',
          }],
        },
      }, {
        id: 2,
        title: 'Task 2',
        lists: {
          list5: [{
            id: 5,
            title: 'Task 5',
          }],
        },
      }],
      list3: [{
        id: 3,
        title: 'Task 3',
      }, {
        id: 5,
        title: 'Task 5',
      }],
      list4: [{
        id: 4,
        title: 'Task 4',
      }, {
        id: 6,
        title: 'Task 6',
      }],
    },
  };
</script>

<main>
  <div id="main">
    <div class="container">
      {#each Object.entries(mb.lists) as [name, taskList]}
        <div class="list">
          <span class="list-title">{name}</span>
          <ul class="list-tasks">
            {#each taskList as task}
              <li class="task">
                <span class="task-title">{task.id}: {task.title}</span>
              </li>
            {/each}
          </ul>
        </div>
      {/each}
    </div>
  </div>
</main>

<style>
  @media (min-width: 640px) {
    .container {
      display: flex;
    }

    .list {
      border: 1px solid black;
      width: 200px;
    }

    .list-tasks {
      list-style-type: none;
      margin-left: 0;
      padding-left: 0;
    }
  
    .task {
      border: 1px solid blue;
      background-color: lightblue;
    }
  }
</style>
