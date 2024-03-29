<script setup>
import EmptyTable from '@/Components/EmptyTable.vue';
import Filter from '@/Components/Filter.vue';
import Pagination from '@/Components/Pagination.vue';
import PrimaryButtonLink from '@/Components/PrimaryButtonLink.vue';
import SelectInput from '@/Components/SelectInput.vue';
import Table from '@/Components/Table.vue';
import Tcell from '@/Components/Tcell.vue';
import TextInput from '@/Components/TextInput.vue';
import AdminLayout from '@/Layouts/AdminLayout.vue';
import { reactive, watch } from 'vue';
import debounce from 'lodash/debounce'
import { router } from '@inertiajs/core';
import TableButton from '@/Components/TableButton.vue';
import {
  CalendarDaysIcon,
  ClipboardDocumentListIcon,
  PencilSquareIcon,
  ClipboardDocumentIcon,
  PlusIcon,
  PrinterIcon
} from '@heroicons/vue/24/outline';



const props = defineProps({
  patients: Object,
  filters: Object,
})

let filters = reactive({
  search: props.filters.search,
  status: props.filters.status,
  created: props.filters.created
})

watch(filters, debounce(function (value) {
  router.get(route('admin.patient'), {
    search: value.search,
    status: value.status,
    created: value.created
  }, {
    preserveScroll: true,
    replace: true
  })
}, 500))
</script>

<template>
  <AdminLayout title="Patients">
    <div class="grid gap-4">
      <div>
        <div class="flex items-center justify-between">
          <div class="flex items-center space-x-3 divide-x-2">
            <TextInput v-model="filters.search" type="search" placeholder="Search..." />
            <div class="flex pl-3 space-x-3">
              <Filter name="Status">
                <SelectInput v-model="filters.status">
                  <option value="">All</option>
                  <option value="pregnant">Pregnant</option>
                </SelectInput>
              </Filter>
              <Filter name="Created">
                <SelectInput v-model="filters.created">
                  <option value="">Oldest</option>
                  <option value="desc">Latest</option>
                </SelectInput>
              </Filter>
            </div>
          </div>
        </div>
      </div>
      <Table :headers="['Full Name', 'Address', 'Contact number', 'Active', 'Actions']">
        <tr v-for="patient in props.patients.data" :key="patient.id">
          <Tcell>{{ patient.full_name }} </Tcell>
          <Tcell>{{ patient.address }} </Tcell>
          <Tcell>{{ patient.contact_number }} </Tcell>
          <Tcell>
            <span :class="{
              'bg-green-100 text-green-500 border-green-400': patient.is_pregnant,
              'bg-red-100 text-red-500 border-red-400': !patient.is_pregnant,
            }" class="px-2 py-0.5 border rounded-full" v-text="patient.is_pregnant ? 'Yes' : 'No'">
            </span>
          </Tcell>
          <Tcell>
            <div class="flex items-center space-x-3">
              <a :href="'/schedule-report/' + patient.id" target="_blank" type="button"
                class="flex items-center font-semibold text-gray-500 uppercase cursor-pointer hover:text-rose-600">
                <PrinterIcon class="h-5 mr-2" />
                Print Schedules
              </a>
            </div>
          </Tcell>
        </tr>
        <EmptyTable v-if="props.patients.data.length == 0" colspan="6" />
        <template #footer>
          <Pagination :prev-url="patients.prev_page_url" :next-url="patients.next_page_url" :from="patients.from"
            :to="patients.to" :total="patients.total" />
        </template>
      </Table>
    </div>
  </AdminLayout>
</template>