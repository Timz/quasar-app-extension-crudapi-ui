<template>
  <q-card bordered class="my-card q-ma-md" flat square>
    <div class="bg-blue-grey-10 text-white">
      <q-toolbar>
        <q-toolbar-title>
          {{ $route.meta.title ?? 'Заголовок страницы не указан в router.meta.title' }}
        </q-toolbar-title>
      </q-toolbar>
      <div class="row items-center bg-blue-grey-9 text-white">
        <crud-button-in-bar v-if="store.getUserCanCreate" label="Создать"
                            @clickBtn="$router.push({ name: store.getEditPageRoute})"></crud-button-in-bar>
        <slot v-if="$slots.buttonsBar" name="buttonsBar"></slot>
        <q-space v-if="$slots['filter-form']"/>
        <q-toggle v-if="$slots['filter-form']" v-model="filterPanel" color="secondary" label="Фильтр"
                  left-label/>
      </div>
    </div>
    <div v-if="$slots['filter-form']" v-show="filterPanel" class="bg-blue-grey-1 q-pa-sm"
         style="border-bottom: #e0e0e0 solid 2px">
      <div class="column">
        <q-form class="row q-gutter-xs">
          <slot name="filter-form"></slot>
          <q-btn class="self-center" color="secondary" label="Сброс" outline unelevated
                 @click="store.resetFilter()"></q-btn>
        </q-form>

      </div>
    </div>
    <q-card-section class="q-pb-none q-pa-none" style="min-height: 80px">
      <slot></slot>
    </q-card-section>
  </q-card>
</template>

<script>
// Вывод списков записей
export default {
  name: 'CrudIndexCard',
  data: function () {
    return {
      filterPanel: false
    }
  },
  inject: ['store'],
  computed: {
    pageListTitle: {
      get: function () {
        return this.$route.meta.title ? this.$route.meta.title : 'Записи'
      }
    }
  },
  methods: {},
}
</script>
