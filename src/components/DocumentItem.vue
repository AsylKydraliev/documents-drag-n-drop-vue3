<template>
  <div class="q-py-md" style="max-width: 1190px">
    <SearchField @search="onSearch"/>

    <q-list bordered class="q-mt-lg">
      <q-expansion-item
          v-for="document in docsSearchResult"
          :key="document.id"
          switch-toggle-side
          expand-separator
          expand-icon-class="text-primary"
          class="expansion"
          draggable="true"
          @dragover.prevent
          @dragenter.prevent
          @drop="onDrop($event, document.id)"
      >
        <template v-slot:header>
          <q-item-section>
            {{ document.title }}
          </q-item-section>

          <q-item-section side>
            <div class="row items-center q-gutter-md">
              <img :src="edit" alt="icon"/>
              <img :src="deleteIcon" alt="icon"/>
              <img :src="dragAndDrop" alt="icon"/>
            </div>
          </q-item-section>
        </template>

        <q-separator/>

        <q-list separator>
          <q-item
              v-for="item in items.filter(i => i.documentId === document.id)"
              :key="item.id"
              clickable
              v-ripple
              draggable="true"
          >
            <q-item-section>{{ item.title }}</q-item-section>
            <q-item-section side>
              <div class="row items-center q-gutter-md">
                <img :src="edit" alt="icon"/>
                <img :src="deleteIcon" alt="icon"/>
                <img
                    :src="dragAndDrop"
                    alt="icon"
                    @dragstart="onDragStart($event, item)"
                />
              </div>
            </q-item-section>
          </q-item>
        </q-list>
      </q-expansion-item>
    </q-list>
  </div>
</template>

<script>
import { onMounted, ref } from 'vue';
import EditIcon from '@/assets/icons/edit.png';
import DeleteIcon from '@/assets/icons/delete.png';
import DragIcon from '@/assets/icons/dragAndDrop.png';
import SearchField from "@/components/SearchFiled.vue";

export default {
  name: 'DocumentItem',
  components: {SearchField},
  setup() {
    const edit = EditIcon;
    const deleteIcon = DeleteIcon;
    const dragAndDrop = DragIcon;

    const currentDragItem = ref(null);
    const items = ref([
      {
        id: 1,
        title: 'Паспорт',
        documentId: 1,
      },
      {
        id: 2,
        title: 'ИНН',
        documentId: 1,
      },
      {
        id: 3,
        title: 'Тестовое задание кандидата',
        documentId: 2,
      },
      {
        id: 4,
        title: 'Трудовой договор кандидата',
        documentId: 2,
      },
      {
        id: 5,
        title: 'Мед. книжка',
        documentId: 3,
      },
      {
        id: 6,
        title: 'Страховка',
        documentId: 3,
      },
    ]);
    const documents = ref([
      {
        id: 1,
        title: 'Обязательные для всех',
      },
      {
        id: 2,
        title: 'Обязательные для трудоустройства',
      },
      {
        id: 3,
        title: 'Специальные',
      },
    ]);
    const docsSearchResult = ref([]);

    onMounted(() => {
      docsSearchResult.value = documents.value;
    })

    const onDragStart = (event, item) => {
      currentDragItem.value = item;
      event.dataTransfer.dropEffect = 'move';
      event.dataTransfer.effectAllowed = 'move';
      event.dataTransfer.setData('itemId', item.id.toString());
    }

    const onDrop = (event, documentId) => {
      const itemId = parseInt(event.dataTransfer.getData('itemId'));
      items.value = items.value.map(item => {
        if (item.id === itemId) {
          item.documentId = documentId;
        }

        return item;
      })
    }

    const onSearch = (value) => {
      if (value === null) {
        return docsSearchResult.value = documents.value;
      }

      docsSearchResult.value = documents.value.filter(doc => {
        return doc.title.toLowerCase().includes(value.toLowerCase())
      });
    }

    return {
      docsSearchResult,
      items,
      edit,
      deleteIcon,
      dragAndDrop,
      onDragStart,
      onDrop,
      onSearch,
    }
  }
}
</script>

<style scoped lang="scss">
.expansion {
  ::v-deep {
    .q-item__section--side > .q-icon {
      border: 1px solid #C2C5CD;
      border-radius: 50%;
    }
  }
}
</style>