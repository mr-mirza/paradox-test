<script setup>
    import { defineProps, reactive, computed } from 'vue'
    import draggable from 'vuedraggable'

    import DocListItem from './DocListItem.vue'




    const exampleData = [
        {
            id: 'required-for-all',
            title: 'Обязательные для всех',
            description: 'Документы, обязательные для всех сотрудников без исключения',
            dots: ['FF238D', 'FFB800', 'FF8D23'],
            isCategory: true
        },
        {
            id: 'passport',
            title: 'Паспорт',
            dots: ['00C2FF'],
            tags: [ { type: 'danger', text: 'Обязательный'}, { type: 'default', text: 'Для всех'}],
            parentId: 'required-for-all'
        },
        {
            id: 'inn',
            title: 'ИНН',
            tags: [ { type: 'danger', text: 'Обязательный'}, { type: 'default', text: 'Для всех'}],
            parentId: 'required-for-all'
        },
        {
            id: 'required-for-jobs',
            title: 'Обязательные для трудоустройства',
            description: 'Документы, без которых невозможно трудоустройство человека на какую бы то ни было должность в компании вне зависимости от гражданства',
            isCategory: true
        },
        {
            id: 'special',
            title: 'Специальные',
            isCategory: true
        },
        {
            id: 'candidate-test',
            title: 'Тестовое задание кандидата',
            description: 'Россия, Белоруссия, Украина, администратор филиала, повар-сушист, повар-пиццмейкер, повар горячего цеха'
        },
        {
            id: 'labor-contract',
            title: 'Трудовой договор',
        },
        {
            id: 'med-book',
            title: 'Мед. книжка',
            dots: ['0066FF', '8E9CBB'],
        },
    ]
    const categoryData = [
        {
            id: 'required-for-all',
            title: 'Обязательные для всех',
            description: 'Документы, обязательные для всех сотрудников без исключения',
            dots: ['FF238D', 'FFB800', 'FF8D23'],
            showItems: true,
            items : [
                {
                    id: 'passport',
                    title: 'Паспорт',
                    dots: ['00C2FF'],
                    tags: [ { type: 'danger', text: 'Обязательный'}, { type: 'default', text: 'Для всех'}],
                },
                {
                    id: 'inn',
                    title: 'ИНН',
                    tags: [ { type: 'danger', text: 'Обязательный'}, { type: 'default', text: 'Для всех'}],
                },
            ]
        },
        {
            id: 'required-for-jobs',
            title: 'Обязательные для трудоустройства',
            description: 'Документы, без которых невозможно трудоустройство человека на какую бы то ни было должность в компании вне зависимости от гражданства',
            showItems: false,
            items: []
        },
        {
            id: 'special',
            title: 'Специальные',
            showItems: false,
            items: []
        },

    ]

    const otherData = {
        items: [
            {
                id: 'candidate-test',
                title: 'Тестовое задание кандидата',
                description: 'Россия, Белоруссия, Украина, администратор филиала, повар-сушист, повар-пиццмейкер, повар горячего цеха'
            },
            {
                id: 'labor-contract',
                title: 'Трудовой договор',
            },
            {
                id: 'med-book',
                title: 'Мед. книжка',
                dots: ['0066FF', '8E9CBB'],
            },
        ]

    }
    const props = defineProps({
        searchKey : {
            type: String
        }
    })

    const state = reactive({
        listData : exampleData,
        categoryData : categoryData,
        otherData : otherData,
        dragTopLevel: false
    })

    const isSearch = computed( () => {
        return props.searchKey.length > 0
    })


    function searchResult() {
        if(!isSearch.value){
            return null
        }
        let ids = []

        for (let cat of state.categoryData){
            if(cat.title.toLowerCase().includes(props.searchKey.toLowerCase()) ){
                ids.push(cat.id)

            }
            for (let subcat of cat.items){
                if(subcat.title.toLowerCase().includes(props.searchKey.toLowerCase()) ){
                    if(!ids.includes(cat.id)){
                        ids.push(cat.id)
                        cat.showItems = true
                    }
                    ids.push(subcat.id)
                }
            }
        }
        for (let otherData of state.otherData.items) {
            if (otherData.title.toLowerCase().includes(props.searchKey.toLowerCase())) {
                ids.push(otherData.id)
            }
        }

        return ids;

        return state.categoryData.filter( (v, key) => {
            return v.title.toLowerCase().includes(props.searchKey.toLowerCase())
        })
    }

    const searchResultComputed = computed( () => {
       return searchResult()
    })


    function inSearch(row) {
        return isSearch.value ? searchResultComputed.value.includes(row.id) : true
    }

    function  handleMove(e) {
        // const { index, futureIndex } = e.draggedContext;
        console.log(e)
    }


</script>
<template>
    <div style="max-width: 1190px">
        <div v-if="isSearch && searchResultComputed.length < 1" class="search-not-found">
            Ничего не найдено. Попробуйте изменить параметры поиска
        </div>
        <template v-else>
            <draggable :dragoverBubble="true"
                       @move="handleMove($event)"
                       :group="{name: 'cat', pull: 'clone', put: false}"
                       @start="state.dragTopLevel = true" @end="state.dragTopLevel = false"
                       class="doc-list-box"  v-model="state.categoryData"  handle=".doc-list-item__btn_handle" item-key="id">
                <template #item="{element}">
                    <div class="doc-list-box__item" :class="{'doc-list-box__item_collapsed': !element.showItems, 'd-none' : !inSearch(element)}">
                        <DocListItem :initData="element" :isCategory="true" @toggle-collapse="element.showItems = !element.showItems"/>

                        <draggable style="position: relative"
                                   :style="{maxHeight: element.showItems ? (element.items.length * 44) + 'px' : 0}"
                                   @change="element.showItems = true"
                                   handle=".doc-list-item__btn_handle"
                                   class="subcategory-list" v-model="element.items" item-key="id" :group="{name: 'subcat', put: !state.dragTopLevel}" >
                            <template #item="{ element: content, index: contentIndex }">
                                <DocListItem
                                        :class="{'d-none' : !inSearch(content)}"
                                        :initData="content" :isHide="!element.showItems"/>
                            </template>
                        </draggable>

                    </div>
                </template>
            </draggable>
            <draggable style="margin-top: 15px"
                       handle=".doc-list-item__btn_handle"
                       class="subcategory-list-free" v-model="state.otherData.items" item-key="id" :group="{name: 'subcat', put: !state.dragTopLevel}" >
                <template #item="{element}">
                    <DocListItem :initData="element"   :class="{'d-none' : !inSearch(element)}" />
                </template>
            </draggable>
        </template>
    </div>
</template>


<style scoped>
.sortable-chosen {
    border-bottom: 4px solid #0066ff;
}
</style>