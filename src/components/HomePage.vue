<template>
  <div
    class="bg-pink-600 text-[#181028] flex flex-col shadow-lg rounded-xl w-full max-w-md overflow-hidden"
  >
    <h1 class="text-2xl text-white text-center font-bold p-4">Restaurants</h1>
    <p class="text-white text-center">Discover restaurants near you</p>
    <ul
      class="relative text-white divide-y group"
      v-for="restaurant in restaurants"
      :key="restaurant"
    >
      <div
        class="absolute right-0 top-1/2 -translate-y-1/2 p-6 flex gap-4 bg-white h-[100%] w-[36%] text-black"
      >
        <button
          class="flex flex-col items-center justify-center hover:opacity-70"
          @click.prevent="handleCall"
        >
          <Call />
          Call
        </button>
        <button
          class="flex flex-col items-center justify-center hover:opacity-70"
        >
          <BookMark />
          Bookmark
        </button>
      </div>
      <li
        class="relative flex gap-6 items-center bg-pink-500 p-4 cursor-pointer transition select-none"
        @mousedown="handleMouseDown"
        @mouseup="handleMouseUp"
        @mousemove="handleMouseMove"
        @touchstart="handleStart"
        @touchend="handleEnd"
        @touchmove="handleMove"
      >
        <img
          class="w-20 h-20 rounded-xl shadow-xl object-cover cursor-pointer"
          :src="restaurant.image"
          :alt="restaurant.alt"
        />
        <div class="flex flex-col space-y-2">
          <p class="text-xl font-medium">{{ restaurant.name }}</p>
          <span class="flex gap-1 text-yellow-400 mb-2">
            <Star
              v-for="(i, index) in 5"
              :key="i"
              :class="{ 'text-gray-700/30': restaurant.rating < index + 1 }"
            />
          </span>
          <div class="flex flex-wrap">
            <div v-for="meal in restaurant.meals" :key="meal" class="mr-2 mb-2">
              <span class="border px-2 py-1 rounded-full cursor-pointer">{{
                meal
              }}</span>
            </div>
          </div>
        </div>
        <div class="ml-auto">
          <span>
            <CornerRight />
          </span>
          <span>{{ restaurant.address }}</span>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { Restaurants } from "../service/interface/Restaurants";
import { ref, Ref } from "vue";
import Star from "../assets/Star.vue";
import CornerRight from "../assets/CornerRight.vue";
import Call from "../assets/CallSvg.vue";
import BookMark from "../assets/BookMark.vue";

const restaurants: Ref<Restaurants[]> = ref([
  {
    name: "Pizza place",
    image:
      "https://plus.unsplash.com/premium_photo-1663858367001-89e5c92d1e0e?q=80&w=1915&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
    meals: ["Breakfast", "Lunch", "Dinner"],
    address: "1.8 km",
    alt: "Monika Grabkowska",
    rating: 3,
  },
  {
    name: "Markus Eatery",
    image:
      "https://media.istockphoto.com/id/1183008025/photo/healthy-veggie-burger.jpg?s=1024x1024&w=is&k=20&c=fFpoEQXm-B9cXhgXMfrwcUMcUHUNnwgmvWTLPmTGFo4=",
    meals: ["Breakfast", "Lunch", "Dinner"],
    address: "2.2 km",
    alt: "",
    rating: 5,
  },
  {
    name: "Fill Me Up",
    image:
      "https://media.istockphoto.com/id/1414285963/photo/stuffed-turkey-for-thanksgiving-holidays-with-pumpkin-peas-pecan-berry-pie-cheese-variations.jpg?s=1024x1024&w=is&k=20&c=oWxa4TLeHit_47M6lGUuPjbcY22HZn-YiyKyXtRYIR8=",
    meals: ["Breakfast", "Lunch", "Dinner"],
    address: "1.8 km",
    alt: "",
    rating: 4,
  },
  {
    name: "George's Delicacy",
    image:
      "https://images.unsplash.com/photo-1565958011703-44f9829ba187?w=600&auto=format&fit=crop&q=60&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MTB8fGZvb2R8ZW58MHx8MHx8fDA%3D",
    meals: ["Breakfast", "Lunch", "Dinner"],
    address: "2.6 km",
    alt: "",
    rating: 3,
  },
  {
    name: "Danny's Diner",
    image:
      "https://images.unsplash.com/photo-1493770348161-369560ae357d?w=600&auto=format&fit=crop&q=60&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MTl8fGZvb2R8ZW58MHx8MHx8fDA%3D",
    meals: ["Breakfast", "Lunch", "Dinner"],
    address: "3.5 km",
    alt: "",
    rating: 2,
  },
]);

const initialX = ref(0);

const isDragging = ref(false);

const maxDragDistance = 160;

const phoneNumber: string = "+233 556956720";

const handleCall = () => {
  window.location.href = "tel:" + phoneNumber;
};

const handleMouseDown = (event: MouseEvent) => {
  initialX.value = event.clientX;
  isDragging.value = true;
  const container = event.currentTarget as HTMLElement;
  container.style.cursor = "grabbing";
};

const handleMouseUp = (event: MouseEvent) => {
  isDragging.value = false;
  const container = event.currentTarget as HTMLElement;
  container.style.cursor = "grab";
};

const handleMouseMove = (event: MouseEvent) => {
  if (!isDragging.value) return;
  const currentX = event.clientX;
  let diff = currentX - initialX.value;

  if (diff > 0) {
    diff = 0;
  } else {
    diff = Math.max(-maxDragDistance, diff);
  }

  const container = event.currentTarget as HTMLElement;
  container.style.transform = `translateX(${diff}px)`;
};

const handleStart = (event: TouchEvent | MouseEvent) => {
  initialX.value =
    event instanceof TouchEvent ? event.touches[0].clientX : event.clientX;
  isDragging.value = true;
};

const handleEnd = () => {
  isDragging.value = false;
};

const handleMove = (event: TouchEvent | MouseEvent) => {
  if (!isDragging.value) return;
  const currentX =
    event instanceof TouchEvent ? event.touches[0].clientX : event.clientX;
  let diff = currentX - initialX.value;

  if (diff > 0) {
    diff = 0;
  } else {
    diff = Math.max(-maxDragDistance, diff);
  }

  const container = event.currentTarget as HTMLElement;
  container.style.transform = `translateX(${diff}px)`;
};
</script>

<style scoped></style>
