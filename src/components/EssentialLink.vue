<template>
  <div class="q-pa-md q-gutter-sm container">
    <q-tree ref="treeRef" :nodes="customize" node-key="id">
      <template v-slot:header-generic="prop">
        <div class="node-content">
          <div>
            <div class="node-title">{{ prop.node.label }}</div>

            <div class="button-row">
              <q-btn round color="primary" icon="mdi-pencil" @click="openEditDialog(prop.node)" class="q-mr-xs" />
              <q-btn color="green" round icon="mdi-plus" @click="addChildNode(prop.node)" class="q-mr-xs" />
              <q-btn round icon="mdi-delete" color="negative" @click="deleteNode(prop.node)" />
            </div>
            
          </div>
        </div>
      </template>
    </q-tree>

    <div class="q-mt-md">
      <q-btn color="green" round icon="mdi-plus" @click="addRootNode" />
    </div>

    <q-dialog v-model="isDialogOpen" persistent>
      <q-card>
        <q-card-section>
          <q-input v-model="editLabel" label="Name" filled autofocus />
        </q-card-section>
        <q-card-actions align="left">
          <q-btn flat label="Cancel" color="negative" @click="closeDialog" />
          <q-btn flat label="Save" color="primary" @click="saveLabel" />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </div>
</template>

<script setup>
import { ref, onMounted, watch, nextTick } from 'vue'
import { QTree, QDialog, QCard, QCardSection, QCardActions, QBtn, QIcon, QInput } from 'quasar'

// Ключ для хранения структуры дерева в localStorage
const STORAGE_KEY = 'tree_structure'

// Состояния и переменные, управляющие деревом и диалогом редактирования
const customize = ref([])
const isDialogOpen = ref(false)
const currentNode = ref(null)
const editLabel = ref('')

// Функция загрузки дерева из localStorage или инициализация по умолчанию
const loadTree = () => {
  const savedTree = localStorage.getItem(STORAGE_KEY)
  if (savedTree) {
    // Если данные сохранены, загружаем их
    customize.value = JSON.parse(savedTree)
  } else {
    // Иначе задаем дерево по умолчанию
    customize.value = [
      {
        id: 1,
        label: 'First',
        header: 'generic',
        isExpanded: true,
        children: [
          {
            id: 2,
            label: 'Good food',
            header: 'generic',
            isExpanded: true,
            children: [
              { id: 3, label: 'Quality ingredients', header: 'generic', isExpanded: true, children: [] }
            ]
          }
        ]
      },
      {
        id: 4,
        label: 'Second',
        header: 'generic',
        isExpanded: true,
        children: [
          {
            id: 5,
            label: 'Good food',
            header: 'generic',
            isExpanded: true,
            children: [
              { id: 6, label: 'Quality ingredients', header: 'generic', isExpanded: true, children: [] }
            ]
          }
        ]
      }
    ]
  }
}


// Функция сохранения текущей структуры дерева в localStorage
const saveTree = () => {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(customize.value))
}

// Загрузка дерева при монтировании компонента
onMounted(loadTree)

// Автоматическое сохранение дерева при изменении customize (глубокое отслеживание)
watch(customize, saveTree, { deep: true })

// Открытие диалога редактирования узла, инициализация значения для редактирования
const openEditDialog = (node) => {
  currentNode.value = node
  editLabel.value = node.label
  isDialogOpen.value = true
}

// Закрытие диалога без сохранения изменений
const closeDialog = () => {
  isDialogOpen.value = false
}

// Сохранение нового имени узла и закрытие диалога
const saveLabel = () => {
  if (currentNode.value) {
    currentNode.value.label = editLabel.value
  }
  closeDialog()
}

// Добавление дочернего узла к заданному родительскому узлу
const addChildNode = (parentNode) => {
  const newNode = {
    id: Date.now(),
    label: 'New Child Node',
    header: 'generic',
    isExpanded: true,
    children: []
  }
  parentNode.isExpanded = true
  if (!parentNode.children) parentNode.children = []
  parentNode.children.push(newNode)
}

// Ссылка на компонент QTree для работы с методами Quasar
const treeRef = ref(null)


// Добавление корневого узла в дерево
const addRootNode = () => {
  const newRoot = {
    id: Date.now(),
    label: 'New Root Node',
    header: 'generic',
    isExpanded: true,
    children: []
  }
  customize.value.push(newRoot)
}

// Функция удаления узла (рекурсивная)
const deleteNode = (node) => {
  const removeNode = (nodes, nodeId) => {
    for (let i = 0; i < nodes.length; i++) {
      if (nodes[i].id === nodeId) {
        // Удаляем узел, если ID совпадает
        nodes.splice(i, 1)
        return true
      } else if (nodes[i].children?.length) {
        // Рекурсивный вызов для дочерних узлов
        if (removeNode(nodes[i].children, nodeId)) return true
      }
    }
  }
  removeNode(customize.value, node.id)
}

</script>


<style scoped lang="scss">
.container {
  background-color: rgb(39, 39, 39);
  height: 100vh;
}

.q-gutter-y-sm, .q-gutter-sm {
    margin-top: 0;
}

.node-content {
  display: flex;
  flex-direction: row;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  background-color: #2d2d2d;
  color: white;
}

.node-title {
  font-weight: bold;
  font-size: 1.1em;
  margin-bottom: 8px;
  color: white;
  text-align: center;
}

.arrow {
  display: flex;
  align-items: center;
  color: white;
}

.button-row {
  display: flex;
  gap: 8px;
}
</style>
