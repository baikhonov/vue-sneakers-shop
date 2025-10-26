<template>
  <header class="flex justify-between px-10 py-8 border-b border-b-[#eaeaea]">
    <div class="flex items-center gap-x-4">
      <img class="w-10" src="/logo.png" alt="Logo" />
      <div>
        <p class="font-bold text-xl uppercase">Vue Sneakers</p>
        <p class="text-sm text-gray-400">Магазин лучших кроссовок</p>
      </div>
    </div>
    <ul class="flex gap-8">
      <li class="flex items-center gap-2 text-gray-600 hover:text-black cursor-pointer">
        <img src="/cart.svg" alt="Корзина" />
        <span class="font-semibold">1205 руб.</span>
      </li>
      <li class="flex items-center gap-2 text-gray-600 hover:text-black cursor-pointer">
        <img src="/heart.svg" alt="Закладки" />
        <span>Закладки</span>
      </li>
      <li class="flex items-center gap-2 text-gray-600 hover:text-black cursor-pointer">
        <img src="/profile.svg" alt="Профиль" />
        <span>Профиль</span>
      </li>
    </ul>
  </header>
</template>
