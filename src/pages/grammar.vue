<script lang="ts" setup>
import axios from 'axios'

let imgSrc = $ref('')
const searchContent = ref('')
const IP = 'http://36.139.161.136:8081'
// const IP = 'http://localhost:8080'
const bookList = $ref([
  { name: '蓝宝书', show: true },
  { name: '考前对策', show: true },
  { name: '完全掌握', show: false },
  { name: '文型辞典', show: false },
])
let blueGrammar = $ref<Grammar[] | null>([])
let preGrammar = $ref<Grammar[] | null>([])
let masterGrammar = $ref<Grammar[] | null>([])
let dicGrammar = $ref<Grammar[] | null>([])
let imgIsLoading = $ref(false)
let selectItemPath = $ref('')

interface Grammar {
  id: number
  title: string
  page: string
  path: string
  level: null | string
}

onMounted(() => {
})

watch(searchContent, async (val) => {
  if (val === '')
    return
  const res = await axios.get(`${IP}/grammar/${val}`)
  blueGrammar = res.data.blue
  preGrammar = res.data.pre
  masterGrammar = res.data.master
  dicGrammar = res.data.dic
})

const itemClick = (item: Grammar) => {
  if (imgSrc.includes(item.path))
    return
  imgIsLoading = true
  imgSrc = `${IP}/file/${item.path}`
  selectItemPath = item.path
}

const imgLoaded = () => {
  imgIsLoading = false
}

const preImg = () => {
  imgIsLoading = true
  const page = Number(selectItemPath.split('(')[1].split(')')[0]) - 1
  const preFix = selectItemPath.split('(')[0]
  const suffix = selectItemPath.split('(')[1].split(')')[1]
  selectItemPath = `${preFix}(${page})${suffix}`
  imgSrc = `${IP}/file/${preFix}(${page})${suffix}`
}

const nextImg = () => {
  imgIsLoading = true
  const page = Number(selectItemPath.split('(')[1].split(')')[0]) + 1
  const preFix = selectItemPath.split('(')[0]
  const suffix = selectItemPath.split('(')[1].split(')')[1]
  selectItemPath = `${preFix}(${page})${suffix}`
  imgSrc = `${IP}/file/${preFix}(${page})${suffix}`
}
</script>

<template>
  <div grid="~ lg:cols-2 gap-2" h-full of-hidden>
    <div h-full grid="~ rows-[min-content_min-content_1fr]" of-hidden>
      <div flex items-center justify-between>
        <div font-bold py-4>
          文法查阅-3406
          <span> v1.0</span>
        </div>
        <div flex gap-2 items-center>
          <div v-for="item in bookList" :key="item.name" cursor-pointer @click="item.show = !item.show">
            <div
              i-carbon-book
              :class="[item.name === '蓝宝书' ? 'text-blue' : item.name === '考前对策' ? 'text-yellow' : item.name === '完全掌握' ? 'text-green' : item.name === '文型辞典' ? 'text-indigo' : '', item.show ? '' : 'grayscale']"
            />
          </div>
        </div>
      </div>
      <div border="~ rounded" shadow z-50>
        <input v-model="searchContent" type="text" w-full p="x-6 y-4" border-none bg-transparent outline-none>
      </div>
      <div border="l b r" h-full mx-2 of-auto z-0>
        <div v-if="bookList[0].show">
          <div
            v-for="item in blueGrammar" :key="item.id" flex text-left items-center px-3 py-2 gap-3 border="b" hover="bg-gray bg-op20"
            @click="itemClick(item)"
          >
            <div v-if="item.level">
              <div
                w-5 h-5 p-1 rounded-sm text-xs flex items-center justify-center text-white
                :class="{ 'bg-red': item.level === 'N1', 'bg-orange': item.level === 'N2', 'bg-yellow': item.level === 'N3', 'bg-blue': item.level === 'N4', 'bg-green': item.level === 'N5' }"
              >
                {{ item.level }}
              </div>
            </div>
            <div>{{ item.title }}</div>
            <div flex-1 />
            <div i-carbon-book text-blue />
          </div>
        </div>

        <div v-if="bookList[1].show">
          <div
            v-for="item in preGrammar" :key="item.id" flex text-left items-center px-3 py-2 gap-3 border="b" hover="bg-gray bg-op20"
            @click="itemClick(item)"
          >
            <div v-if="item.level">
              <div
                w-5 h-5 p-1 rounded-sm text-xs flex items-center justify-center text-white
                :class="{ 'bg-red': item.level === 'N1', 'bg-orange': item.level === 'N2', 'bg-yellow': item.level === 'N3', 'bg-blue': item.level === 'N4', 'bg-green': item.level === 'N5' }"
              >
                {{ item.level }}
              </div>
            </div>
            <div>{{ item.title }}</div>
            <div flex-1 />
            <div i-carbon-book text-yellow />
          </div>
        </div>

        <div v-if="bookList[2].show">
          <div
            v-for="item in masterGrammar" :key="item.id" flex text-left items-center px-3 py-2 gap-3 border="b" hover="bg-gray bg-op20"
            @click="itemClick(item)"
          >
            <div v-if="item.level">
              <div
                w-5 h-5 p-1 rounded-sm text-xs flex items-center justify-center text-white
                :class="{ 'bg-red': item.level === 'N1', 'bg-orange': item.level === 'N2', 'bg-yellow': item.level === 'N3', 'bg-blue': item.level === 'N4', 'bg-green': item.level === 'N5' }"
              >
                {{ item.level }}
              </div>
            </div>
            <div>{{ item.title }}</div>
            <div flex-1 />
            <div i-carbon-book text-indigo />
          </div>
        </div>

        <div v-if="bookList[3].show">
          <div
            v-for="item in dicGrammar" :key="item.id" flex text-left items-center px-3 py-2 gap-3 border="b" hover="bg-gray bg-op20"
            @click="itemClick(item)"
          >
            <div v-if="item.level">
              <div
                w-5 h-5 p-1 rounded-sm text-xs flex items-center justify-center text-white
                :class="{ 'bg-red': item.level === 'N1', 'bg-orange': item.level === 'N2', 'bg-yellow': item.level === 'N3', 'bg-blue': item.level === 'N4', 'bg-green': item.level === 'N5' }"
              >
                {{ item.level }}
              </div>
            </div>
            <div>{{ item.title }}</div>
            <div flex-1 />
            <div i-carbon-book text-green />
          </div>
        </div>
      </div>
    </div>
    <div v-if="imgSrc !== ''" h-full w-full relative of-hidden>
      <div absolute left-0 top="1/2" w-10 h-10 rounded-full bg="black op40" hover="bg-op90" op40 flex justify-center items-center @click="preImg">
        <div i-carbon-arrow-left text-white />
      </div>
      <div absolute right-0 top="1/2" w-10 h-10 rounded-full bg="black op40" hover="bg-op90" op40 flex justify-center items-center @click="nextImg">
        <div i-carbon-arrow-right text-white />
      </div>
      <div v-if="imgIsLoading" w-full h-full bg-black absolute op80 flex items-center justify-center>
        <div i-eos-icons-loading text-white w-10 h-10 />
      </div>
      <div w-full h-full of-auto>
        <img object-contain w-full :src="imgSrc" @load="imgLoaded">
      </div>
    </div>
  </div>
</template>
