<template>
  <main>
    <Table class="w-[80%] mx-auto mt-6">
      <TableHeader>
        <TableRow>
          <TableHead class="w-[50px]"></TableHead>
          <TableHead>Название</TableHead>
          <TableHead>Бюджет</TableHead>
          <TableHead>Статус</TableHead>
          <TableHead>Ответственный</TableHead>
          <TableHead>Дата создания</TableHead>
        </TableRow>
      </TableHeader>
      <TableBody>
        <TableRow v-for="(order, index) in fakeDataBase" :key="index">
          <TableCell>
            <DropdownMenu>
              <DropdownMenuTrigger>
                <Button>+</Button>
              </DropdownMenuTrigger>
              <DropdownMenuContent>
                <DropdownMenuLabel>Контакты клиента</DropdownMenuLabel>
                <DropdownMenuSeparator />
                <DropdownMenuItem class="font-bold">
                  {{ order.contactName }}
                </DropdownMenuItem>
                <DropdownMenuItem>
                  <a v-if="order.links.phone" class="user-link" :href="`tel:${order.links.phone}`">
                    <img class="user-link__img" src="/img/icons/phone_icon.svg" alt="" />
                  </a>
                  <a
                    v-if="order.links.email"
                    class="user-link"
                    :href="`mailto:${order.links.email}`"
                  >
                    <img class="user-link__img" src="/img/icons/mail_icon.svg" alt="" />
                  </a>
                </DropdownMenuItem>
              </DropdownMenuContent>
            </DropdownMenu>
          </TableCell>
          <TableCell class="w-[150px]">Дело #{{ order.ID }} </TableCell>
          <TableCell>{{ `${order.budget}${actualCurrency(order.currency)}` }}</TableCell>
          <TableCell>
            <Badge class="uppercase cursor-default" :class="checkStatus(order.status)">{{
              order.status
            }}</Badge>
          </TableCell>
          <TableCell>{{ order.managerName }}</TableCell>
          <TableCell> {{ order.date }} </TableCell>
        </TableRow>
      </TableBody>
    </Table>
  </main>
</template>

<script setup lang="ts">
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow
} from '@/components/ui/table'
import { Button } from '@/components/ui/button'
import { Badge } from '@/components/ui/badge'
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuLabel,
  DropdownMenuSeparator,
  DropdownMenuTrigger
} from '@/components/ui/dropdown-menu'

type DataItemLinks = {
  phone: Number
  email: String
}

type DataItemStatus = 'Заявка' | 'Ожидание ответа' | 'В работе' | 'Завершено' | 'Отказ'
type DataItemCurrency = 'USD' | 'RUB' | 'EUR'

type DataItem = {
  ID: Number
  managerName: String
  contactName: String
  links: DataItemLinks
  date: String
  budget: Number
  currency: DataItemCurrency
  status: DataItemStatus
}

const fakeDataBase: Array<DataItem> = [
  {
    ID: 0,
    managerName: 'Конор Макгрегор',
    contactName: 'Нейт Диаз',
    links: {
      email: 'natediaz@gmail.com',
      phone: 99998887766
    },
    date: '20.08.2016',
    budget: 30000000,
    currency: 'USD',
    status: 'Завершено'
  },
  {
    ID: 1,
    managerName: 'Лео Месси',
    contactName: 'Криштиану Роналду',
    links: {
      email: 'cr7@gmail.com',
      phone: 22222222222
    },
    date: '22.06.2024',
    budget: 999900000,
    currency: 'EUR',
    status: 'В работе'
  },
  {
    ID: 2,
    managerName: 'Килиан Мбаппе',
    contactName: 'Флорентино Перес',
    links: {
      email: 'ucl@gmail.com',
      phone: 12223334455
    },
    date: '01.07.2024',
    budget: 60000000,
    currency: 'EUR',
    status: 'Ожидание ответа'
  }
]

const actualCurrency = (arg: DataItemCurrency) => {
  if (arg === 'EUR') {
    return '€'
  }
  if (arg === 'USD') {
    return '$'
  }
  if (arg === 'RUB') {
    return '₽'
  }
  return '?'
}

const checkStatus = (arg: DataItemStatus) => {
  if (arg === 'Заявка') {
    return `bg-yellow-400 text-black`
  }
  if (arg === 'Ожидание ответа') {
    return `bg-blue-500`
  }
  if (arg === 'В работе') {
    return `bg-lime-600`
  }
  if (arg === 'Завершено') {
    return `bg-gray-400`
  }
  if (arg === 'Отказ') {
    return `bg-red-400`
  }
  return `bg-black`
}
</script>

<style src="./HomeView.css" scoped />
