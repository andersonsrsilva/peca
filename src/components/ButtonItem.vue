<template>
  <button
    aria-current="true"
    type="button"
    class="inline-block leading-tight hover:bg-opacity-5 text-left px-6 py-2 border-b border-gray-200 w-full rounded-t-lg bg-blue-600 cursor-pointer flex items-center hover:bg-yellow-light focus:bg-yellow-light"
  >
    <div
      class="h-10 w-10 bg-contain bg-no-repeat bg-left text-red"
      :class="icon"
    ></div>

    <div class="text-xs font-regular text-red uppercase ml-10">{{ name }}</div>
    <div class="text-xs font-regular text-red uppercase ml-auto">
      {{ $t('DOWNLOAD') }}
    </div>
  </button>
</template>

<script>
export default {
  props: {
    name: { type: String, required: true },
    icon: { type: String, required: true },
  },
}
</script>
