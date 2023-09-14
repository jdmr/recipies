<template>
    <div
        class="container mx-auto max-w-prose p-8 min-h-screen flex flex-col items-center justify-center"
    >
        <h1 class="text-5xl tracking-widest mb-8">RECIPIES</h1>
        <div class="flex gap-1">
            <input
                id="recipie"
                v-model="recipie.name"
                type="text"
                class="rounded-lg text-gray-800 p-2 text-xl tracking-wider"
                @keyup.enter="addRecipie"
            />
            <button
                class="bg-gray-100 text-gray-800 py-2 px-4 rounded-lg text-xl tracking-wider flex items-center shadow shadow-pink-500 hover:shadow-lg hover:shadow-pink-700 transition-all"
                @click="addRecipie"
            >
                <i class="i-mdi:plus h-6 w-6"></i>
                <div>ADD</div>
            </button>
        </div>
        <ul class="flex flex-col gap-2 mt-8 w-full">
            <li
                v-for="rec in recipies"
                :key="rec.id"
                class="bg-pink-300 text-gray-800 text-4xl tracking-wider p-4 rounded-lg shadow flex justify-between"
            >
                <div class="w-full p-2">
                    <div class="text-6xl mb-4">{{ rec.name }}</div>
                    <div>
                        <ul class="flex flex-col gap-1">
                            <li
                                v-for="item in rec.items"
                                :key="item.id"
                                class="flex justify-between"
                            >
                                <div>{{ item.name }}</div>
                                <div>{{ item.amount }}</div>
                                <div>
                                    <button @click="removeItem(item)">
                                        <i class="i-mdi:close"></i>
                                    </button>
                                </div>
                            </li>
                        </ul>
                    </div>
                    <div class="flex flex-col">
                        <label :for="rec.id + 'name'">Item Name</label>
                        <input :id="rec.id + 'name'" type="text" />
                    </div>
                    <div class="flex flex-col">
                        <label :for="rec.id + 'amount'">Item Amount</label>
                        <input :id="rec.id + 'amount'" type="text" />
                    </div>
                    <div class="grid">
                        <button
                            class="bg-purple-500 text-white mt-4 rounded-full"
                            @click="addItem"
                        >
                            Add Item
                        </button>
                    </div>
                </div>
                <button
                    class="bg-pink-800 text-pink-100 rounded-full flex items-center p-2 hover:bg-pink-900 transition-all"
                    @click="deleteRecipie(rec.id)"
                >
                    <i class="i-mdi:delete"></i>
                </button>
            </li>
        </ul>
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

interface Item {
    id: string
    name: string
    amount: string
}

interface Recipie {
    id: string
    name: string
    items: Item[]
}

const recipies = ref([] as Recipie[])
const recipie = ref({} as Recipie)
const selected = ref({} as Recipie)

const addRecipie = () => {
    selected.value = {
        id: Math.random().toString(),
        name: recipie.value.name,
        items: []
    }
    recipies.value.push(selected.value)
    recipie.value.name = ''
    const input = document.getElementById('recipie') as HTMLInputElement
    input.focus()
}
const deleteRecipie = (recipieID: string) => {
    recipies.value = recipies.value.filter((rec) => rec.id !== recipieID)
}

const addItem = () => {
    console.log('getting name: ', selected.value.id + 'name')
    const nameInput = document.getElementById(
        selected.value.id + 'name'
    ) as HTMLInputElement
    const amountInput = document.getElementById(
        selected.value.id + 'amount'
    ) as HTMLInputElement
    if (!nameInput && !amountInput) {
        alert('Please provide an item name and amount')
        return
    }
    if (!selected.value.items) {
        selected.value.items = []
    }
    selected.value.items.push({
        id: Math.random().toString(),
        name: nameInput.value,
        amount: amountInput.value
    } as Item)
    nameInput.value = ''
    amountInput.value = ''
}

const removeItem = (item: Item) => {
    selected.value.items = selected.value.items.filter((i) => i.id !== item.id)
}
</script>
