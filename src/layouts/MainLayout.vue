<template>
  <q-layout view="lHh Lpr lFf">
    <q-page-container>
      <RouterView v-slot="{ Component }">
        <template v-if="Component">
          <transition name="fade">
            <Suspense>
              <!-- main content -->
              <component :is="Component" :key="$route.fullPath"></component>

              <!-- loading state -->
              <template #fallback>
                <div v-if="error">Please keep calm, we are fixing servers</div>
                <div v-else>
                  <VnLoading />
                </div>
              </template>
            </Suspense>
          </transition>
        </template>
      </RouterView>
    </q-page-container>
  </q-layout>
  <VnFooter />
</template>

<script setup lang="ts">
import { useQuasar } from 'quasar';

import { useMeta } from 'quasar';

useMeta({
  title: 'VueNpm - Only VueJs. All the VueJs packages at one place',
  meta: {
    description: {
      name: 'description',
      content:
        'Optimize your Vue.js development workflow with VueNpm, your comprehensive resource for finding and utilizing Vue.js packages.',
    },
  },
});

const error = ref<boolean>(false);
const $q = useQuasar();

onErrorCaptured((e, instance, info) => {
  console.log(e, instance, info);

  error.value = true;

  $q.notify({
    color: 'negative',
    message: `Error: ${e.message} in ${info}`,
    icon: 'report_problem',
  });
});
</script>
