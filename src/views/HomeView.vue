<script setup lang="ts">
import BaseLayout from '@/layouts/BaseLayout.vue';
import { columns, rows } from '@/content/table-data';
import { vIntersectionObserver } from '@vueuse/components';
import { reactive } from 'vue';

type TableIds =
  | 'horizontalScrollTable'
  | 'horizontalTableWithHeight'
  | 'stickyHeaderTable'
  | 'stickyHeaderAndColumnTable';

type TableVisibility = Record<TableIds, boolean>;

const tableVisiblity: TableVisibility = reactive({
  horizontalScrollTable: false,
  horizontalTableWithHeight: false,
  stickyHeaderTable: false,
  stickyHeaderAndColumnTable: false,
});

function onIntersectionObserver(e: IntersectionObserverEntry[]) {
  const id = e[0].target.id as TableIds;
  tableVisiblity[id] = e[0].isIntersecting;
}
</script>

<template>
  <div>
    <BaseLayout>
      <template #sidebar>
        <div class="h-full flex flex-col gap-8 items-center justify-center">
          <RouterLink
            :to="{ path: '/', hash: '#horizontalScrollTable' }"
            :class="tableVisiblity.horizontalScrollTable ? 'activeLink' : 'link'"
            >Horizontally Scrollable Table</RouterLink
          >
          <RouterLink
            to="/#horizontalTableWithHeight"
            :class="tableVisiblity.horizontalTableWithHeight ? 'activeLink' : 'link'"
            >Horizontally Scrollable Table With Fixed Height</RouterLink
          >
          <RouterLink
            to="/#stickyHeaderTable"
            :class="tableVisiblity.stickyHeaderTable ? 'activeLink' : 'link'"
            >Sticky Header Table
          </RouterLink>
          <RouterLink
            to="/#stickyHeaderAndColumnTable"
            :class="tableVisiblity.stickyHeaderAndColumnTable ? 'activeLink' : 'link'"
            >Sticky Header And Column Table
          </RouterLink>
        </div>
      </template>

      <template #main>
        <div>
          <!-- region: horizontally scrollable table -->
          <section id="horizontalScrollTable" v-intersection-observer="onIntersectionObserver">
            <h2>Horizontal Scroll Table</h2>

            <!-- region: table -->
            <div class="overflow-x-auto mt-10">
              <table>
                <thead>
                  <tr>
                    <th v-for="column in columns" :key="column.accessor">{{ column.label }}</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="row in rows" :key="row.id">
                    <td v-for="column in columns" :key="column.accessor">
                      {{ row[column.accessor] }}
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
            <!-- endregion: table -->
          </section>
          <!-- endregion: horizontally scrollable table -->

          <!-- region: horizontally scrollable table with fixed height -->
          <div id="horizontalTableWithHeight" v-intersection-observer="onIntersectionObserver">
            <h2>Horizontal Table With Height</h2>

            <!-- region: table -->
            <div class="overflow-x-auto h-[40rem] mt-10">
              <table>
                <thead>
                  <tr>
                    <th v-for="column in columns" :key="column.accessor">{{ column.label }}</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="row in rows" :key="row.id">
                    <td v-for="column in columns" :key="column.accessor">
                      {{ row[column.accessor] }}
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
            <!-- endregion: table -->
          </div>
          <!-- endregion: horizontally scrollable table with fixed height -->

          <!-- region: sticky header table-->
          <div id="stickyHeaderTable" v-intersection-observer="onIntersectionObserver">
            <h2>Sticky Header Table</h2>

            <!-- region: table -->
            <div class="overflow-x-auto h-[40rem] mt-10">
              <table>
                <thead class="sticky top-0 z-30">
                  <tr>
                    <th v-for="column in columns" :key="column.accessor">{{ column.label }}</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="row in rows" :key="row.id">
                    <td v-for="column in columns" :key="column.accessor">
                      {{ row[column.accessor] }}
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
            <!-- endregion: table -->
          </div>
          <!-- endregion: sticky header table-->

          <!-- region: sticky header and column table-->
          <div id="stickyHeaderAndColumnTable" v-intersection-observer="onIntersectionObserver">
            <h2>Sticky Header And Column Table</h2>

            <!-- region: table -->
            <div class="overflow-x-auto h-[40rem] mt-10">
              <table>
                <thead class="sticky top-0 z-30">
                  <tr>
                    <th
                      :class="column.sticky && 'lg:sticky left-0 z-30'"
                      v-for="column in columns"
                      :key="column.accessor"
                    >
                      {{ column.label }}
                    </th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="row in rows" :key="row.id">
                    <td
                      v-for="column in columns"
                      :key="column.accessor"
                      :class="column.sticky && 'positionSticky left-0 z-20'"
                    >
                      {{ row[column.accessor] }}
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
            <!-- endregion: table -->
          </div>
          <!-- endregion: sticky header table-->
        </div>
      </template>
    </BaseLayout>
  </div>
</template>

<style scoped lang="postcss">
h2 {
  @apply text-4xl font-bold text-indigo-500 text-center mt-8 capitalize;
}

table {
  @apply border-separate mx-auto;
}

tr {
  @apply even:bg-slate-50;
}

tr:nth-child(even) td.positionSticky {
  @apply bg-slate-50 lg:sticky;
}

tr:nth-child(odd) td.positionSticky {
  @apply bg-white lg:sticky;
}

th {
  @apply text-left bg-slate-100;
}

th,
td {
  @apply p-4  whitespace-nowrap border border-slate-200;
}

.link {
  @apply font-semibold text-slate-500 text-lg text-center;
}

.activeLink {
  @apply font-bold text-indigo-500 text-lg text-center;
}
</style>
