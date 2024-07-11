<template>
  <BContainer class="py-2 px-2 mx-0" :fluid="true">
    <div class="d-flex mb-4">
      <BInputGroup class="w-25 d-flex">
        <BFormInput
          id="input-1"
          placeholder="Введите имя"
          v-model="filter"
          type="search"
        />
        <BInputGroupAppend>
          <BButton
            variant="dark"
            :disabled="!filter"
            @click="filter = ''"
            class="rounded-start-0 rounded-end"
            >Clear</BButton
          >
        </BInputGroupAppend>
      </BInputGroup>
    </div>
    <BTable
    hover
      class="mt-1 rounded-3 order-table"
      model="sortBy"
      :sort-internal="true"
      :items="itemsTyped"
      :fields="fieldsTyped"
      :filter="filter"
      :responsive="false"
      :filterable="filterOn"
      :multisort="true"
      :stickyHeader="true"
      @filtered="onFiltered"
    >
      <template #cell(actions)="row">
        <BButton
          class="rounded cirlce"
          variant="dark"
          @click="row.toggleDetails"
        >
        {{ row.detailsShowing ? "Скрыть" : "Показать" }}
    </BButton>
      </template>
      <template #row-details="row">
        <table class="details dark-subtle">
          <tbody>
            <td class="px-2">
              {{ row.item.contactName }}
            </td>
            <td class="px-2">
              <a v-bind:href="'tel:'+row.item.phoneNumber">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="1.25em"
                  height="1.25em"
                  fill="currentColor"
                  class="bi bi-telephone"
                  viewBox="0 0 16 16"
                >
                  <path
                    d="M3.654 1.328a.678.678 0 0 0-1.015-.063L1.605 2.3c-.483.484-.661 1.169-.45 1.77a17.6 17.6 0 0 0 4.168 6.608 17.6 17.6 0 0 0 6.608 4.168c.601.211 1.286.033 1.77-.45l1.034-1.034a.678.678 0 0 0-.063-1.015l-2.307-1.794a.68.68 0 0 0-.58-.122l-2.19.547a1.75 1.75 0 0 1-1.657-.459L5.482 8.062a1.75 1.75 0 0 1-.46-1.657l.548-2.19a.68.68 0 0 0-.122-.58zM1.884.511a1.745 1.745 0 0 1 2.612.163L6.29 2.98c.329.423.445.974.315 1.494l-.547 2.19a.68.68 0 0 0 .178.643l2.457 2.457a.68.68 0 0 0 .644.178l2.189-.547a1.75 1.75 0 0 1 1.494.315l2.306 1.794c.829.645.905 1.87.163 2.611l-1.034 1.034c-.74.74-1.846 1.065-2.877.702a18.6 18.6 0 0 1-7.01-4.42 18.6 18.6 0 0 1-4.42-7.009c-.362-1.03-.037-2.137.703-2.877z"
                  />
                </svg>
              </a>
            </td>

            <td>
              <a v-bind:href="'mailto:'+ row.item.email">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="1.25em"
                  height="1.25em"
                  fill="currentColor"
                  class="bi bi-envelope"
                  viewBox="0 0 16 16"
                >
                  <path
                    d="M0 4a2 2 0 0 1 2-2h12a2 2 0 0 1 2 2v8a2 2 0 0 1-2 2H2a2 2 0 0 1-2-2zm2-1a1 1 0 0 0-1 1v.217l7 4.2 7-4.2V4a1 1 0 0 0-1-1zm13 2.383-4.708 2.825L15 11.105zm-.034 6.876-5.64-3.471L8 9.583l-1.326-.795-5.64 3.47A1 1 0 0 0 2 13h12a1 1 0 0 0 .966-.741M1 11.105l4.708-2.897L1 5.383z"
                  />
                </svg>
              </a>
            </td>
          </tbody>
        </table>
      </template>
    </BTable>
  </BContainer>
</template>

<script setup lang="ts">
import {
  BButton,
  BInputGroup,
  BFormInput,
  BContainer,
  BTable,
  type TableFieldRaw,
  type TableItem,
} from "bootstrap-vue-next";
import { ref } from "vue";

interface lead {
  id: number;
  name: string;
  budget: string;
  state: string;
  assignee: string;
  createdAt: string;
  contactName: string;
  phoneNumber: string;
  email: string;
}
const fieldsTyped: Exclude<TableFieldRaw<lead>, string>[] = [
  {
    key: "name",
    label: "Название",
    sortable: true,
    sortDirection: "desc",
    class: "text-md",
  },
  {
    key: "budget",
    label: "Бюджет",
    sortable: true,
    sortDirection: "desc",
    class: "text-md",
  },
  {
    key: "state",
    label: "Статус",
    sortable: true,
    sortDirection: "desc",
    class: "text-md",
  },
  {
    key: "assignee",
    label: "Ответственный",
    sortable: true,
    sortDirection: "desc",
    class: "text-md",
  },
  {
    key: "createdAt",
    label: "Дата создания",
    sortable: true,
    sortDirection: "desc",
    class: "text-md",
  },
  {
    key: "actions",
    label: "",
    sortable: false,
    class: "text-md",
  },
];

const itemsTyped: lead[] = [
  {
    id: 1,
    name: "Заявка 1",
    budget: "10000",
    state: "Новая",
    assignee: "Иванов",
    createdAt: "2022-01-01",
    contactName: "Скрудж",
    phoneNumber: "+7 (999) 999-99-99",
    email: "john@example.com",
  },
];

function onFiltered(filteredItems: TableItem<lead>[]) {
  totalRows.value = filteredItems.length;
}


const totalRows = ref(itemsTyped.length);
const filter = ref("");
const filterOn = ref([]);
</script>

<style>
.order-table {
  overflow-y: auto;
  overflow-x: hidden;

  scrollbar-width: thin;
  scrollbar-color: rgba(107, 113, 118, 1) rgba(255, 255, 255, 0);
}
.details td:first-child {
  border-right: 1px solid #adb5bd; /* Add a border to the right of the first cell */
}
</style>
