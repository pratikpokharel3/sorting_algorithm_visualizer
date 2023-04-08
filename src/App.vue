<script setup>
import { ref } from "vue";
import { algorithms } from "./algorithms";

const nums = ref([]);
const size = ref(15);
const ms = ref(100);
const algorithmsList = ref(algorithms);
const selectedAlgorithm = ref("bubble_sort");
const isSortingComplete = ref(true);

initialize();

function initialize() {
  nums.value = [];

  for (let i = 1; i <= size.value; i++) {
    nums.value.push(Math.ceil(Math.random() * 100));
  }
}

function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function insertionSort() {
  isSortingComplete.value = false;

  let i, key, j;

  for (i = 1; i < nums.value.length; i++) {
    await delay(ms.value);

    key = nums.value[i];
    j = i - 1;

    while (j >= 0 && nums.value[j] > key) {
      nums.value[j + 1] = nums.value[j];
      j = j - 1;
    }
    nums.value[j + 1] = key;
  }

  isSortingComplete.value = true;
}

async function selectionSort() {
  isSortingComplete.value = false;

  let i, j, min_idx;

  for (i = 0; i < nums.value.length - 1; i++) {
    await delay(ms.value);

    min_idx = i;

    for (j = i + 1; j < nums.value.length; j++) {
      if (nums.value[j] < nums.value[min_idx]) {
        min_idx = j;
      }
    }

    let temp = nums.value[min_idx];
    nums.value[min_idx] = nums.value[i];
    nums.value[i] = temp;
  }

  isSortingComplete.value = true;
}

async function bubbleSort() {
  isSortingComplete.value = false;

  const size = nums.value.length;

  for (let i = 0; i < size - 1; i++) {
    for (let j = 0; j < size - i - 1; j++) {
      if (nums.value[j] > nums.value[j + 1]) {
        await delay(ms.value);

        const temp = nums.value[j];
        nums.value[j] = nums.value[j + 1];
        nums.value[j + 1] = temp;
      }
    }
  }

  isSortingComplete.value = true;
}

async function heapSort() {
  isSortingComplete.value = false;

  let N = nums.value.length;

  for (let i = Math.floor(N / 2) - 1; i >= 0; i--) {
    heapify(N, i);
  }

  for (let i = N - 1; i > 0; i--) {
    await delay(ms.value);

    let temp = nums.value[0];
    nums.value[0] = nums.value[i];
    nums.value[i] = temp;

    heapify(i, 0);
  }

  isSortingComplete.value = true;
}

function heapify(N, i) {
  let largest = i;
  let l = 2 * i + 1;
  let r = 2 * i + 2;

  if (l < N && nums.value[l] > nums.value[largest]) {
    largest = l;
  }

  if (r < N && nums.value[r] > nums.value[largest]) {
    largest = r;
  }

  if (largest != i) {
    let swap = nums.value[i];
    nums.value[i] = nums.value[largest];
    nums.value[largest] = swap;

    heapify(N, largest);
  }
}

function handleSorting() {
  if (selectedAlgorithm.value === "insertion_sort") {
    insertionSort();
  } else if (selectedAlgorithm.value === "selection_sort") {
    selectionSort();
  } else if (selectedAlgorithm.value === "bubble_sort") {
    bubbleSort();
  } else {
    heapSort();
  }
}
</script>

<template>
  <nav class="navbar bg-dark" data-bs-theme="dark">
    <div class="container">
      <span class="navbar-brand">Sorting Algorithm Visualizer</span>

      <div>
        <a
          href="https://github.com/pratikpokharel3/sorting_algorithm_visualizer"
          target="_blank"
        >
          <svg
            viewBox="0 0 24 24"
            class="github_link"
            xmlns="http://www.w3.org/2000/svg"
          >
            <title>github</title>
            <path
              d="M12,2A10,10 0 0,0 2,12C2,16.42 4.87,20.17 8.84,21.5C9.34,21.58 9.5,21.27 9.5,21C9.5,20.77 9.5,20.14 9.5,19.31C6.73,19.91 6.14,17.97 6.14,17.97C5.68,16.81 5.03,16.5 5.03,16.5C4.12,15.88 5.1,15.9 5.1,15.9C6.1,15.97 6.63,16.93 6.63,16.93C7.5,18.45 8.97,18 9.54,17.76C9.63,17.11 9.89,16.67 10.17,16.42C7.95,16.17 5.62,15.31 5.62,11.5C5.62,10.39 6,9.5 6.65,8.79C6.55,8.54 6.2,7.5 6.75,6.15C6.75,6.15 7.59,5.88 9.5,7.17C10.29,6.95 11.15,6.84 12,6.84C12.85,6.84 13.71,6.95 14.5,7.17C16.41,5.88 17.25,6.15 17.25,6.15C17.8,7.5 17.45,8.54 17.35,8.79C18,9.5 18.38,10.39 18.38,11.5C18.38,15.32 16.04,16.16 13.81,16.41C14.17,16.72 14.5,17.33 14.5,18.26C14.5,19.6 14.5,20.68 14.5,21C14.5,21.27 14.66,21.59 15.17,21.5C19.14,20.16 22,16.42 22,12A10,10 0 0,0 12,2Z"
            />
          </svg>
        </a>

        <button
          type="button"
          class="btn d-sm-none"
          aria-controls="offcanvas"
          data-bs-toggle="offcanvas"
          data-bs-target="#offcanvas"
        >
          <svg
            fill="white"
            viewBox="0 0 24 24"
            xmlns="http://www.w3.org/2000/svg"
          >
            <title>Options</title>
            <path d="M3,6H21V8H3V6M3,11H21V13H3V11M3,16H21V18H3V16Z" />
          </svg>
        </button>

        <div
          tabindex="-1"
          id="offcanvas"
          aria-labelledby="offcanvas"
          class="offcanvas offcanvas-end"
        >
          <div class="offcanvas-header">
            <h4 class="offcanvas-title">Options</h4>

            <button
              type="button"
              class="btn-close"
              aria-label="Close"
              data-bs-dismiss="offcanvas"
            ></button>
          </div>
          <div class="offcanvas-body">
            <div>Sorting Algorithm</div>

            <select
              class="form-select form-select-sm mt-2"
              aria-label="Select Sorting Algorithm"
              v-model="selectedAlgorithm"
            >
              <option
                v-for="item in algorithmsList"
                :key="item.id"
                :value="item.id"
                :selected="selectedAlgorithm.id === item.id"
              >
                {{ item.name }}
              </option>
            </select>

            <label for="size_range" class="form-label mt-4">
              Size: <span class="fw-bold">{{ size }}</span>
            </label>

            <input
              min="5"
              max="20"
              step="1"
              type="range"
              id="size_range"
              class="form-range"
              :disabled="!isSortingComplete"
              v-model="size"
              @input="initialize"
            />

            <label for="delay_range" class="form-label mt-4">
              Delay: <span class="fw-bold">{{ ms }} ms</span>
            </label>

            <input
              min="50"
              max="500"
              step="50"
              type="range"
              id="delay_range"
              class="form-range"
              :disabled="!isSortingComplete"
              v-model="ms"
            />
          </div>
        </div>
      </div>
    </div>
  </nav>

  <div class="container">
    <div class="row g-0 justify-content-sm-between mt-4">
      <div class="col-12 col-sm-7 col-md-8 col-lg-10 text-center">
        <button
          type="button"
          class="btn btn-sm btn-dark"
          :disabled="!isSortingComplete"
          @click="initialize"
        >
          Reset
        </button>

        <button
          type="button"
          class="btn btn-sm btn-dark ms-2"
          :disabled="!isSortingComplete"
          @click="handleSorting"
        >
          Play/Sort
        </button>

        <div class="mt-3">
          <div>Selected Algorithm:</div>
          <span class="fw-bold">
            {{
              selectedAlgorithm
                .replace("_", " ")
                .replace(/(?: |\b)(\w)/g, function (key, p1) {
                  return key.toUpperCase();
                })
            }}
          </span>
        </div>

        <div class="d-flex gap-2 justify-content-center align-items-end mt-5">
          <div
            class="bg-dark"
            v-for="i in nums"
            :style="{
              width: '20px',
              height: i + 250 + 'px'
            }"
          ></div>
        </div>
      </div>

      <div
        class="col-6 col-sm-4 col-md-3 col-lg-2 d-none d-sm-block border-start ps-3 mt-3"
      >
        <select
          class="form-select form-select-sm"
          aria-label="Select Sorting Algorithm"
          v-model="selectedAlgorithm"
        >
          <option
            v-for="item in algorithmsList"
            :key="item.id"
            :value="item.id"
            :selected="selectedAlgorithm.id === item.id"
          >
            {{ item.name }}
          </option>
        </select>

        <label for="size_range" class="form-label mt-4">
          Size: <span class="fw-bold">{{ size }}</span>
        </label>

        <input
          min="5"
          max="20"
          step="1"
          type="range"
          id="size_range"
          class="form-range"
          :disabled="!isSortingComplete"
          v-model="size"
          @input="initialize"
        />

        <label for="delay_range" class="form-label mt-4">
          Delay: <span class="fw-bold">{{ ms }} ms</span>
        </label>

        <input
          min="50"
          max="500"
          step="50"
          type="range"
          id="delay_range"
          class="form-range"
          :disabled="!isSortingComplete"
          v-model="ms"
        />
      </div>
    </div>
  </div>
</template>

<style scoped>
svg {
  width: 24px;
  height: 24px;
}

.github_link {
  fill: rgba(235, 235, 235, 0.6);
}

.github_link:hover {
  fill: #e1e1e1;
  cursor: pointer;
}
</style>
