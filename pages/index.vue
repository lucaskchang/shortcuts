<template>
  <div class="mx-auto min-h-screen p-4 text-gray-950">
    <UInput
      v-model="input"
      size="xl"
      autofocus
      placeholder="Type something..."
      icon="i-mdi-search"
      :ui="{ icon: { trailing: { pointer: '' } } }"
    >
      <template #trailing>
        <UButton
          v-show="input !== ''"
          color="gray"
          variant="link"
          icon="i-heroicons-x-mark"
          :padded="false"
          @click="input = ''"
        />
      </template>
    </UInput>
    <div class="mt-4 flex flex-wrap">
      <div
        v-for="shortcut in results"
        :key="shortcut.name"
        class="w-1/2 sm:w-1/3 md:w-1/4"
      >
        <a
          v-if="shortcut.type === 'link'"
          :href="shortcut.url"
          target="_blank"
          rel="noopener noreferrer"
        >
          <UCard class="m-2 cursor-pointer text-wrap transition duration-300 ease-in-out hover:ring-2 hover:ring-yellow-400">
            <div class="flex flex-row items-center space-x-2">
              <UIcon
                name="i-mdi-link"
                class="size-5"
              />
              <p>
                {{ shortcut.name }}
              </p>
            </div>
            <p class="text-gray-600">
              {{ shortcut.url }}
            </p>
          </UCard>
        </a>
        <div v-else>
          <UCard
            class="hover:ring-primary-500 m-2 cursor-pointer text-wrap transition duration-300 ease-in-out hover:ring-2"
            @click="copySnippet(shortcut.snippet)"
          >
            <div class="flex flex-row items-center space-x-2">
              <UIcon
                name="i-mdi-content-copy"
                class="size-5"
              />
              <p>
                {{ shortcut.name }}
              </p>
            </div>
            <p class="text-gray-600">
              {{ shortcut.snippet }}
            </p>
          </UCard>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import shortcuts from '~/assets/shortcuts.json';

const toast = useToast();

function copySnippet(snippet: string) {
  navigator.clipboard.writeText(snippet);
  toast.add({
    title: 'Copied to clipboard',
    icon: 'i-mdi-clipboard-check',
    description: snippet,
  });
}

const input = ref('');
const results = computed(() => {
  return shortcuts.filter(shortcut =>
    shortcut.name.toLowerCase().includes(input.value.toLowerCase())
    || shortcut.snippet?.toLowerCase().includes(input.value.toLowerCase())
    || shortcut.url?.toLowerCase().includes(input.value.toLowerCase()),
  );
});
</script>
