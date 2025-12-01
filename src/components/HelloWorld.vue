<template>
  <main class="md:container md:mx-auto w-full pt-[4%]">
    <h1 class="text-3xl font-bold mb-2">Planner</h1>

    <div
      v-if="data"
      class="grid grid-cols-2 md:grid-cols-4 xl:grid-cols-8 gap-4"
    >
      <div class="card planner-daily col-span-2 md:col-span-4 lg:col-span-2">
        <h2 class="text-xl font-bold">Daily Tasks</h2>

        <div class="grid grid-cols-8">
          <span class="col-span-6">Resets in {{ nextDayFormatted }}</span>
          <div class="col-span-2 text-end">
            <button @click="resetTracker('daily')">Reset</button>
          </div>
        </div>

        <div v-for="(game, key) of data.daily" :key="index" class="mt-5">
          <h3 class="text-md font-semibold">
            {{ game.title }}
          </h3>

          <div v-for="(task, index) in game.items" :key="index" class="flex">
            <input
              type="checkbox"
              v-model="task.state"
              :id="'daily-' + key + '-' + index"
              @change="updateTaskState(key, index, 'daily', $event)"
            />
            <label
              :for="'daily-' + key + '-' + index"
              class="grid grid-cols-8 ms-2 w-full"
              :class="{ striked: task.state }"
            >
              <span class="col-span-7">
                {{ task.name }}
              </span>
              <span class="col-span-1" @click="deleteTask(key, index, 'daily')"
                >x</span
              >
            </label>
          </div>

          <input
            type="text"
            class="w-full input-xs mt-3"
            @keydown.enter="addTask(key, 'daily', $event)"
            placeholder="New task..."
          />

          <hr />
        </div>
      </div>

      <div class="card planner-weekly col-span-2 md:col-span-4 lg:col-span-2">
        <h2 class="text-xl font-bold">Weekly Tasks</h2>

        <div class="grid grid-cols-8">
          <span class="col-span-6">Resets in {{ nextMondayFormatted }}</span>
          <div class="col-span-2 text-end">
            <button @click="resetTracker('weekly')">Reset</button>
          </div>
        </div>

        <div v-for="(game, key) of data.weekly" :key="index" class="mt-5">
          <h3 class="text-md font-semibold">
            {{ game.title }}
          </h3>

          <div v-for="(task, index) in game.items" :key="index" class="flex">
            <input
              type="checkbox"
              v-model="task.state"
              :id="'weekly-' + key + '-' + index"
              @change="updateTaskState(key, index, 'weekly', $event)"
            />
            <label
              :for="'weekly-' + key + '-' + index"
              class="grid grid-cols-8 ms-2 w-full"
              :class="{ striked: task.state }"
            >
              <span class="col-span-7">
                {{ task.name }}
              </span>
              <span class="col-span-1" @click="deleteTask(key, index, 'weekly')"
                >x</span
              >
            </label>
          </div>

          <input
            type="text"
            class="w-full input-xs mt-3"
            @keydown.enter="addTask(key, 'weekly', $event)"
            placeholder="New task..."
          />

          <hr />
        </div>
      </div>

      <div class="card planner-monthly col-span-2 md:col-span-4 lg:col-span-2">
        <h2 class="text-xl font-bold">Monthly Tasks</h2>

        <div class="grid grid-cols-8">
          <span class="col-span-6">Resets in {{ nextMonthFormatted }}</span>
          <div class="col-span-2 text-end">
            <button @click="resetTracker('monthly')">Reset</button>
          </div>
        </div>

        <div v-for="(game, key) of data.monthly" :key="index" class="mt-5">
          <h3 class="text-md font-semibold">
            {{ game.title }}
          </h3>

          <div v-for="(task, index) in game.items" :key="index" class="flex">
            <input
              type="checkbox"
              v-model="task.state"
              :id="'monthly-' + key + '-' + index"
              @change="updateTaskState(key, index, 'monthly', $event)"
            />
            <label
              :for="'monthly-' + key + '-' + index"
              class="grid grid-cols-8 ms-2 w-full"
              :class="{ striked: task.state }"
            >
              <span class="col-span-7">
                {{ task.name }}
              </span>
              <span
                class="col-span-1"
                @click="deleteTask(key, index, 'monthly')"
                >x</span
              >
            </label>
          </div>

          <input
            type="text"
            class="w-full input-xs mt-3"
            @keydown.enter="addTask(key, 'monthly', $event)"
            placeholder="New task..."
          />

          <hr />
        </div>
      </div>

      <div class="card planner-todo col-span-2 md:col-span-4 lg:col-span-2">
        <h2 class="text-xl font-bold">Todos</h2>
        <div v-for="(game, key) of data.todo" :key="index" class="mt-5">
          <h3 class="text-md font-semibold">
            {{ game.title }}
          </h3>

          <div v-for="(task, index) in game.items" :key="index" class="flex">
            <input
              type="checkbox"
              v-model="task.state"
              :id="'todo-' + key + '-' + index"
              @change="updateTaskState(key, index, 'todo', $event)"
            />
            <label
              :for="'todo-' + key + '-' + index"
              class="grid grid-cols-8 ms-2 w-full"
              :class="{ striked: task.state }"
            >
              <span class="col-span-7">
                {{ task.name }}
              </span>
              <span class="col-span-1" @click="deleteTask(key, index, 'todo')"
                >x</span
              >
            </label>
          </div>

          <input
            type="text"
            class="w-full input-xs mt-3"
            @keydown.enter="addTask(key, 'todo', $event)"
            placeholder="New task..."
          />

          <hr />
        </div>
      </div>
    </div>

    <div class="mt-10">
      <h3 class="text-xl font-bold">Track game</h3>

      <select
        class="select"
        name="trackGame"
        id="trackGame"
        @change="addNewGameToPlanner('daily', $event)"
      >
        <option
          v-for="(newGame, index) of gamesList"
          :key="index"
          :disabled="data.daily[newGame.key]"
          :value="newGame.key"
        >
          {{ newGame.title }}
        </option>
      </select>
    </div>

    <div class="mt-10">
      <h3 class="text-xl font-bold">Untrack game</h3>

      <select
        class="select"
        name="untrackGame"
        id="untrackGame"
        @change="deleteGameFromPlanner($event)"
      >
        <option
          v-for="(newGame, index) of gamesList"
          :key="index"
          :disabled="!data.daily[newGame.key]"
          :value="newGame.key"
        >
          {{ newGame.title }}
        </option>
      </select>
    </div>
  </main>
</template>

<script setup>
import { ref, onMounted, onUnmounted, computed } from 'vue';

defineProps({
  msg: String,
});

const count = ref(0);
const data = ref({
  daily: {
    genshin: {
      title: 'Genshin',
      items: [],
    },
    zzz: {
      title: 'Zenless Zone Zero',
      items: [],
    },
    'wuthering-waves': {
      title: 'Wuthering Waves',
      items: [],
    },
  },
  weekly: {
    genshin: {
      title: 'Genshin',
      items: [],
    },
    zzz: {
      title: 'Zenless Zone Zero',
      items: [],
    },
    'wuthering-waves': {
      title: 'Wuthering Waves',
      items: [],
    },
  },
  monthly: {
    genshin: {
      title: 'Genshin',
      items: [],
    },
    zzz: {
      title: 'Zenless Zone Zero',
      items: [],
    },
    'wuthering-waves': {
      title: 'Wuthering Waves',
      items: [],
    },
  },
  todo: {
    genshin: {
      title: 'Genshin',
      items: [],
    },
    zzz: {
      title: 'Zenless Zone Zero',
      items: [],
    },
    'wuthering-waves': {
      title: 'Wuthering Waves',
      items: [],
    },
  },
});

const gamesList = [
  { title: 'Genshin', key: 'genshin' },
  { title: 'Zenless Zone Zero', key: 'zzz' },
  { title: 'Wuthering Waves', key: 'wuthering-waves' },
  { title: 'Honkai: Star Rail', key: 'honkai-star-rail' },
  { title: 'Reverse: 1999', key: 'reverse-1999' },
  { title: 'Duet Night Abyss', key: 'duet-night-abyss' },
  { title: 'Marvel Rivals', key: 'rivals' },
  { title: 'Umamusume', key: 'umamusume' },
  { title: "Girl's Frontline 2", key: 'gfl2' },
  { title: 'Overwatch 2', key: 'overwatch2' },
  { title: 'Arknights: Endfield', key: 'endfield' },
];

const tick = ref(Date.now());

let timer;

onMounted(() => {
  if (localStorage.getItem('plannerData')) {
    data.value = JSON.parse(localStorage.getItem('plannerData'));
  } else {
    localStorage.setItem('plannerData', JSON.stringify(data.value));
  }

  timer = setInterval(() => {
    tick.value = Date.now();
  }, 1000);
});

onUnmounted(() => clearInterval(timer));

// ###### computed ########
const nextMondayCountdown = computed(() => {
  const now = new Date(tick.value);
  const day = now.getDay(); // Monday = 1

  const daysUntilMonday = (8 - day) % 7 || 7;

  const next = new Date(now);
  next.setDate(now.getDate() + daysUntilMonday);
  next.setHours(4, 0, 0, 0);

  return next - now;
});

const nextMondayFormatted = computed(() => format(nextMondayCountdown.value));

const nextDayCountdown = computed(() => {
  const now = new Date(tick.value);

  const next = new Date(now);
  next.setDate(now.getDate() + 1);
  next.setHours(4, 0, 0, 0);

  return next - now;
});

const nextDayFormatted = computed(() => format(nextDayCountdown.value));

const nextMonthCountdown = computed(() => {
  const now = new Date(tick.value);

  const currentYear = now.getFullYear();
  const currentMonth = now.getMonth();

  // jump to next month
  const next = new Date(currentYear, currentMonth + 1, 1, 4, 0, 0, 0);

  return next - now;
});

const nextMonthFormatted = computed(() => format(nextMonthCountdown.value));

// ###### methods ########
const format = (ms) => {
  const days = Math.floor(ms / 86400000);
  ms -= days * 86400000;

  const hours = Math.floor(ms / 3600000);

  return `${days}d ${hours}h`;
};

const addTask = (key, area, event) => {
  const taskName = event.target.value;
  if (taskName == undefined) return;
  if (taskName.trim() == '') return;

  if (taskName) {
    if (area == 'daily')
      data.value.daily[key].items.push({ name: taskName, state: false });
    else if (area == 'weekly')
      data.value.weekly[key].items.push({ name: taskName, state: false });
    else if (area == 'monthly')
      data.value.monthly[key].items.push({ name: taskName, state: false });
    else if (area == 'todo')
      data.value.daily[key].items.push({ name: taskName, state: false });

    localStorage.setItem('plannerData', JSON.stringify(data.value));
    event.target.value = '';
    event.atrget.blur();
  }
};

const updateTaskState = (key, index, area, event) => {
  if (area == 'daily') {
    data.value.daily[key].items[index].state = event.target.checked;
  } else if (area == 'weekly') {
    data.value.weekly[key].items[index].state = event.target.checked;
  } else if (area == 'monthly') {
    data.value.monthly[key].items[index].state = event.target.checked;
  } else if (area == 'todo') {
    data.value.todo[key].items[index].state = event.target.checked;
  }

  localStorage.setItem('plannerData', JSON.stringify(data.value));
};

const addNewGameToPlanner = (area, event) => {
  let gameKey = event.target.value;
  let gameTitle = '';
  gamesList.forEach((game) => {
    if (game.key == gameKey) {
      gameTitle = game.title;
    }
  });

  // if the game doesn't exist in the planner, add it
  if (!data.value.daily[gameKey]) {
    data.value.daily[gameKey] = { title: gameTitle, items: [] };
    data.value.weekly[gameKey] = { title: gameTitle, items: [] };
    data.value.monthly[gameKey] = { title: gameTitle, items: [] };
    data.value.todo[gameKey] = { title: gameTitle, items: [] };
  }

  localStorage.setItem('plannerData', JSON.stringify(data.value));
};

const deleteGameFromPlanner = (event) => {
  let gameKey = event.target.value;
  if (!gameKey) return;

  delete data.value.daily[gameKey];
  delete data.value.weekly[gameKey];
  delete data.value.monthly[gameKey];
  delete data.value.todo[gameKey];

  localStorage.setItem('plannerData', JSON.stringify(data.value));
};

const deleteTask = (key, index, area) => {
  if (area == 'daily') {
    data.value.daily[key].items.splice(index, 1);
  } else if (area == 'weekly') {
    data.value.weekly[key].items.splice(index, 1);
  } else if (area == 'monthly') {
    data.value.monthly[key].items.splice(index, 1);
  } else if (area == 'todo') {
    data.value.todo[key].items.splice(index, 1);
  }

  localStorage.setItem('plannerData', JSON.stringify(data.value));
};

const resetTracker = (area) => {
  // turn every state in the respective area into false, without deleting any properties
  if (area == 'daily') {
    // for (const game of data.value.daily) {
    //   game.items.forEach((item) => {
    //     item.state = false;
    //   });
    // }

    for (const game in data.value.daily) {
      data.value.daily[game].items.forEach((item) => {
        item.state = false;
      })
    }
  }

  if (area == 'weekly') {
    // for (const game of data.value.weekly) {
    //   game.items.forEach((item) => {
    //     item.state = false;
    //   });
    // }

    for (const game in data.value.weekly) {
      data.value.weekly[game].items.forEach((item) => {
        item.state = false;
      })
    }
  }
  if (area == 'monthly') {
    // for (const game of data.value.monthly) {
    //   game.items.forEach((item) => {
    //     item.state = false;
    //   });
    // }

    for (const game in data.value.monthly) {
      data.value.monthly[game].items.forEach((item) => {
        item.state = false;
      })
    }
  }

  localStorage.setItem('plannerData', JSON.stringify(data.value));
};
</script>

<style scoped>
.read-the-docs {
  color: #888;
}

.striked {
  text-decoration: line-through;
  opacity: 0.4;
}
</style>
