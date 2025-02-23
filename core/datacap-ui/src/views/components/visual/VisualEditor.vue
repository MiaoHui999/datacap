<template>
  <div class="flex h-full space-x-1 mt-2">
    <div class="left flex-1 justify-center">
      <CircularLoading v-if="loading" :show="loading"/>
      <div v-else>
        <Alert v-if="configuration?.headers.length === 0 && !configuration?.message" class="mt-20" :title="$t('dataset.tip.adhocDnd')"/>
        <Alert v-else-if="configuration?.message" class="mt-20" :title="configuration.message" type="error"/>
        <div v-else>
          <VisualTable v-if="configuration?.type === Type.TABLE" :configuration="configuration" @change="handlerCommit"/>
          <VisualLine v-else-if="configuration?.type === Type.LINE" :configuration="configuration" @change="handlerCommit"/>
          <VisualBar v-else-if="configuration?.type === Type.BAR" :configuration="configuration" @change="handlerCommit"/>
          <VisualArea v-else-if="configuration?.type === Type.AREA" :configuration="configuration" @change="handlerCommit"/>
          <VisualPie v-else-if="configuration?.type === Type.PIE" :configuration="configuration" @change="handlerCommit"/>
          <VisualHistogram v-else-if="configuration?.type === Type.HISTOGRAM" :configuration="configuration" @change="handlerCommit"/>
          <VisualWordCloud v-else-if="configuration?.type === Type.WORDCLOUD" :configuration="configuration" @change="handlerCommit"/>
          <VisualScatter v-else-if="configuration?.type === Type.SCATTER" :configuration="configuration" @change="handlerCommit"/>
          <VisualRadar v-else-if="configuration?.type === Type.RADAR" :configuration="configuration" @change="handlerCommit"/>
          <VisualFunnel v-else-if="configuration?.type === Type.FUNNEL" :configuration="configuration" @change="handlerCommit"/>
          <VisualGauge v-else-if="configuration?.type === Type.GAUGE" :configuration="configuration" @change="handlerCommit"/>
        </div>
      </div>
    </div>
    <div class="right w-[210px] space-y-2">
      <Card body-class="p-2">
        <template #title>{{ $t('dataset.common.visualType') }}</template>
        <CircularLoading v-if="loading" :show="loading"/>
        <div v-else-if="configuration">
          <ToggleGroup v-model="configuration.type" type="single">
            <div class="grid grid-cols-4 items-center space-x-1 space-y-1">
              <ToggleGroupItem class="mt-1" :disabled="configuration.headers.length === 0" :value="Type.TABLE">
                <Tooltip :content="$t('dataset.common.visualTypeTable')">
                  <Table :size="20"/>
                </Tooltip>
              </ToggleGroupItem>
              <ToggleGroupItem :disabled="configuration.headers.length === 0" :value="Type.LINE">
                <Tooltip :content="$t('dataset.common.visualTypeLine')">
                  <svg xmlns="http://www.w3.org/2000/svg" width="30" height="24" viewBox="0 0 24 24" fill="none" class="vchart-dropdown-content-item-icon" id="lineChart"
                       style="width: 24px;">
                    <path d="M4 4.5V18.5C4 19.0523 4.44772 19.5 5 19.5H20" stroke="#21252C" stroke-width="1.8" stroke-linecap="round"></path>
                    <path d="M8 14.5L12.4982 9.91029C12.5716 9.83537 12.6889 9.82567 12.7737 9.88752L15.812 12.1051C15.8933 12.1644 16.0051 12.1582 16.0793 12.0903L20 8.5"
                          stroke="#21252C" stroke-width="1.8" stroke-linecap="round"></path>
                  </svg>
                </Tooltip>
              </ToggleGroupItem>
              <ToggleGroupItem :disabled="configuration.headers.length === 0" :value="Type.BAR">
                <Tooltip :content="$t('dataset.common.visualTypeBar')">
                  <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" class="vchart-dropdown-content-item-icon" id="barChart"
                       style="width: 24px;">
                    <path d="M4 4.5V18.5C4 19.0523 4.44772 19.5 5 19.5H20" stroke="#21252C" stroke-width="1.8" stroke-linecap="round"></path>
                    <path d="M13 7V16" stroke="#21252C" stroke-width="1.8" stroke-linecap="round"></path>
                    <path d="M8.5 10V16" stroke="#21252C" stroke-width="1.8" stroke-linecap="round"></path>
                    <path d="M17.5 12V16" stroke="#21252C" stroke-width="1.8" stroke-linecap="round"></path>
                  </svg>
                </Tooltip>
              </ToggleGroupItem>
              <ToggleGroupItem :disabled="configuration.headers.length === 0" :value="Type.AREA">
                <Tooltip :content="$t('dataset.common.visualTypeArea')">
                  <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" class="vchart-dropdown-content-item-icon" id="areaChart"
                       style="width: 24px;">
                    <path
                        d="M18 20H6C4.89543 20 4 19.1046 4 18V9L8.71327 5.70071C8.88543 5.5802 9.11457 5.5802 9.28673 5.70071L13.7201 8.8041C13.8889 8.92223 14.1128 8.92478 14.2842 8.81051L19.2226 5.51823C19.5549 5.29672 20 5.53491 20 5.93426V18C20 19.1046 19.1046 20 18 20Z"
                        stroke="#21252C" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"></path>
                    <path d="M20 11L14 14.5L9 11L4 14.5" stroke="#21252C" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"></path>
                  </svg>
                </Tooltip>
              </ToggleGroupItem>
              <ToggleGroupItem :disabled="configuration.headers.length === 0" :value="Type.PIE">
                <Tooltip :content="$t('dataset.common.visualTypePie')">
                  <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 20 20" fill="none" class="vchart-dropdown-content-item-icon" id="pieChart"
                       style="width: 24px;">
                    <path d="M6.26599 3.38867C3.46047 4.60563 1.5 7.38359 1.5 10.6159C1.5 14.97 5.0576 18.4998 9.44612 18.4998C12.6024 18.4998 15.3288 16.674 16.6111 14.0288"
                          stroke="#21252C" stroke-opacity="0.9" stroke-width="1.8" stroke-linecap="round"></path>
                    <path
                        d="M18.4993 9.88932C18.4405 5.28275 14.7173 1.55949 10.1107 1.50071C10.0498 1.49993 10 1.54934 10 1.61021V9.44897C10 9.7533 10.2467 10 10.551 10H18.3898C18.4507 10 18.5001 9.95018 18.4993 9.88932Z"
                        stroke="#21252C" stroke-opacity="0.9" stroke-width="1.8"></path>
                  </svg>
                </Tooltip>
              </ToggleGroupItem>
              <ToggleGroupItem :disabled="configuration.headers.length === 0" :value="Type.HISTOGRAM">
                <Tooltip :content="$t('dataset.common.visualTypeHistogram')">
                  <svg xmlns="http://www.w3.org/2000/svg" width="21" height="20" viewBox="0 0 21 20" fill="none">
                    <g clip-path="url(#clip0_1700_69225)">
                      <path
                          d="M11.1663 3.33331H9.83301C9.28072 3.33331 8.83301 3.78103 8.83301 4.33331V15.6666C8.83301 16.2189 9.28072 16.6666 9.83301 16.6666H11.1663C11.7186 16.6666 12.1663 16.2189 12.1663 15.6666V4.33331C12.1663 3.78103 11.7186 3.33331 11.1663 3.33331Z"
                          stroke="#21252C" stroke-width="1.3" stroke-linecap="round" stroke-linejoin="round"></path>
                      <path
                          d="M5.33333 7.5H4C3.44772 7.5 3 7.94772 3 8.5V15.6667C3 16.219 3.44771 16.6667 4 16.6667H5.33333C5.88562 16.6667 6.33333 16.2189 6.33333 15.6667V8.5C6.33333 7.94772 5.88562 7.5 5.33333 7.5Z"
                          stroke="#21252C" stroke-width="1.3" stroke-linecap="round" stroke-linejoin="round"></path>
                      <path
                          d="M17.0003 9.16669H15.667C15.1147 9.16669 14.667 9.6144 14.667 10.1667V15.6667C14.667 16.219 15.1147 16.6667 15.667 16.6667H17.0003C17.5526 16.6667 18.0003 16.219 18.0003 15.6667V10.1667C18.0003 9.6144 17.5526 9.16669 17.0003 9.16669Z"
                          stroke="#21252C" stroke-width="1.3" stroke-linecap="round" stroke-linejoin="round"></path>
                    </g>
                    <defs>
                      <clipPath id="clip0_1700_69225">
                        <rect width="20" height="20" fill="white" transform="translate(0.5)"></rect>
                      </clipPath>
                    </defs>
                  </svg>
                </Tooltip>
              </ToggleGroupItem>
              <ToggleGroupItem :disabled="configuration.headers.length === 0" :value="Type.WORDCLOUD">
                <Tooltip :content="$t('dataset.common.visualTypeWordCloud')">
                  <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" class="vchart-dropdown-content-item-icon" id="wordCloud"
                       style="width: 24px;">
                    <path d="M5.81563 7V8.77143H3.92339V15H2.39681V8.77143H0.5V7H5.81563Z" fill="#21252C"></path>
                    <path d="M11.4096 13.28V15H6.50986V7H11.3913V8.72H8.03645V10.1029H11.1947V11.8171H8.03645V13.28H11.4096Z" fill="#21252C"></path>
                    <path d="M17.972 7L15.9015 10.96L17.9948 15H16.2443L14.9645 12.4L13.6847 15H11.9525L14.0458 10.96L11.9799 7H13.7304L14.9828 9.54857L16.2351 7H17.972Z"
                          fill="#21252C"></path>
                    <path d="M23.5 7V8.77143H21.6078V15H20.0812V8.77143H18.1844V7H23.5Z" fill="#21252C"></path>
                    <path
                        d="M4.94095 17.8V18.73H3.69895V22H2.69695V18.73H1.45195V17.8H4.94095ZM8.61263 21.097V22H5.39663V17.8H8.60063V18.703H6.39863V19.429H8.47163V20.329H6.39863V21.097H8.61263ZM12.92 17.8L11.561 19.879L12.935 22H11.786L10.946 20.635L10.106 22H8.96897L10.343 19.879L8.98697 17.8H10.136L10.958 19.138L11.78 17.8H12.92ZM16.5484 17.8V18.73H15.3064V22H14.3044V18.73H13.0594V17.8H16.5484Z"
                        fill="#21252C"></path>
                    <path
                        d="M13.6175 1.5V2.275H12.5825V5H11.7475V2.275H10.71V1.5H13.6175ZM16.6772 4.2475V5H13.9972V1.5H16.6672V2.2525H14.8322V2.8575H16.5597V3.6075H14.8322V4.2475H16.6772ZM20.2666 1.5L19.1341 3.2325L20.2791 5H19.3216L18.6216 3.8625L17.9216 5H16.9741L18.1191 3.2325L16.9891 1.5H17.9466L18.6316 2.615L19.3166 1.5H20.2666ZM23.2903 1.5V2.275H22.2553V5H21.4203V2.275H20.3828V1.5H23.2903Z"
                        fill="#21252C"></path>
                  </svg>
                </Tooltip>
              </ToggleGroupItem>
              <ToggleGroupItem :disabled="configuration.headers.length === 0" :value="Type.SCATTER">
                <Tooltip :content="$t('dataset.common.visualTypeScatter')">
                  <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" class="vchart-dropdown-content-item-icon" id="scatterChart"
                       style="width: 24px;">
                    <path d="M4 4.5V18.5C4 19.0523 4.44772 19.5 5 19.5H20" stroke="#21252C" stroke-width="1.8" stroke-linecap="round"></path>
                    <circle cx="8.5" cy="15.5" r="1.5" fill="#21252C"></circle>
                    <circle cx="16" cy="7" r="2" fill="#21252C"></circle>
                    <circle cx="18" cy="15" r="2" fill="#21252C"></circle>
                    <circle cx="12.75" cy="12.25" r="2.25" fill="#21252C"></circle>
                  </svg>
                </Tooltip>
              </ToggleGroupItem>
              <ToggleGroupItem :disabled="configuration.headers.length === 0" :value="Type.RADAR">
                <Tooltip :content="$t('dataset.common.visualTypeRadar')">
                  <svg width="22" height="21" viewBox="0 0 22 21" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path
                        d="M1.57045 8.16646L10.3949 1.45986C10.7525 1.18808 11.2475 1.18808 11.6051 1.45986L20.4296 8.16646C20.7706 8.42565 20.9087 8.87328 20.7729 9.27956L17.4187 19.3169C17.2824 19.7249 16.9004 20 16.4703 20H5.52971C5.09956 20 4.7176 19.7249 4.58127 19.3169L1.22709 9.27956C1.09132 8.87328 1.2294 8.42565 1.57045 8.16646Z"
                        stroke="#21252C" stroke-width="1.7" stroke-linecap="round"></path>
                    <path
                        d="M6.11243 9.82863L10.8826 6.2478C10.951 6.19642 11.0446 6.19428 11.1153 6.24249L16.3379 9.80252C16.4279 9.8639 16.4522 9.98606 16.3926 10.0773L13.5468 14.4281C13.5169 14.4739 13.4696 14.5054 13.4158 14.5153L7.91428 15.5328C7.81444 15.5513 7.71661 15.492 7.68675 15.395L6.04134 10.0474C6.01654 9.96678 6.04498 9.87927 6.11243 9.82863Z"
                        stroke="#21252C" stroke-width="1.7" stroke-linecap="round"></path>
                  </svg>
                </Tooltip>
              </ToggleGroupItem>
              <ToggleGroupItem :disabled="configuration.headers.length === 0" :value="Type.FUNNEL">
                <Tooltip :content="$t('dataset.common.visualTypeFunnel')">
                  <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" class="vchart-dropdown-content-item-icon" id="funnelChart"
                       style="width: 24px;">
                    <path
                        d="M10.7172 18.1334C11.3011 19.0968 12.6989 19.0968 13.2828 18.1334L20.6197 6.02745C21.2256 5.0278 20.5058 3.75 19.3369 3.75H4.66307C3.49415 3.75 2.77442 5.02779 3.38027 6.02745L10.7172 18.1334Z"
                        stroke="#21252C" stroke-width="1.8"></path>
                    <path d="M4.52637 8.25H19.5264" stroke="#21252C" stroke-width="1.8"></path>
                    <path d="M7.52637 12.25H16.5264" stroke="#21252C" stroke-width="1.8"></path>
                  </svg>
                </Tooltip>
              </ToggleGroupItem>
              <ToggleGroupItem :disabled="configuration.headers.length === 0" :value="Type.GAUGE">
                <Tooltip :content="$t('dataset.common.visualTypeGauge')">
                  <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" class="vchart-dropdown-content-item-icon" id="gauge"
                       style="width: 24px;">
                    <circle cx="12" cy="12" r="9.5" stroke="#21252C" stroke-width="1.8"></circle>
                    <circle cx="12" cy="16" r="2" stroke="#21252C" stroke-width="1.8"></circle>
                    <path d="M12.9 7C12.9 6.50294 12.4971 6.1 12 6.1C11.5029 6.1 11.1 6.50294 11.1 7H12.9ZM11.1 7V14H12.9V7H11.1Z" fill="#21252C"></path>
                  </svg>
                </Tooltip>
              </ToggleGroupItem>
            </div>
          </ToggleGroup>
        </div>
      </Card>
      <Card body-class="p-2">
        <template #title>{{ $t('dataset.common.visualConfigure') }}</template>
        <CircularLoading v-if="loading" :show="loading"/>
        <div v-else-if="configuration" class="flex items-center justify-center">
          <Alert v-if="configuration.type === Type.TABLE" :title="$t('dataset.common.visualConfigureNotSpecified')"/>
          <Button v-else size="sm" class="w-[80%]" @click="configureVisible = true">{{ $t('common.configure') }}</Button>
        </div>
      </Card>
    </div>
  </div>
  <VisualConfigure v-if="configureVisible && configuration" :is-visible="configureVisible" :configuration="configuration" :field-group="forwardFiled(configuration.type)"
                   @close="configureVisible = $event" @change="configuration.chartConfigure = $event"/>
</template>
<script lang="ts">
import { Type } from '@/views/components/visual/Type'
import VisualWordCloud from '@/views/components/visual/components/VisualWordCloud.vue'
import VisualHistogram from '@/views/components/visual/components/VisualHistogram.vue'
import VisualPie from '@/views/components/visual/components/VisualPie.vue'
import VisualArea from '@/views/components/visual/components/VisualArea.vue'
import { ChartFieldGroup, Configuration } from './Configuration'
import VisualBar from '@/views/components/visual/components/VisualBar.vue'
import VisualLine from '@/views/components/visual/components/VisualLine.vue'
import VisualTable from '@/views/components/visual/components/VisualTable.vue'
import { defineComponent, PropType } from 'vue'
import CircularLoading from '@/views/components/loading/CircularLoading.vue'
import { Table } from 'lucide-vue-next'
import { RadioGroup, RadioGroupItem } from '@/components/ui/radio-group'
import { ToggleGroup, ToggleGroupItem } from '@/components/ui/toggle-group'
import Card from '@/views/ui/card'
import Tooltip from '@/views/ui/tooltip'
import Alert from '@/views/ui/alert'
import VisualScatter from '@/views/components/visual/components/VisualScatter.vue'
import VisualConfigure from '@/views/components/visual/components/VisualConfigure.vue'
import VisualRadar from '@/views/components/visual/components/VisualRadar.vue'
import VisualFunnel from '@/views/components/visual/components/VisualFunnel.vue'
import VisualGauge from '@/views/components/visual/components/VisualGauge.vue'
import Button from '@/views/ui/button'
import { createdConfigure } from '@/views/components/visual/Utils.ts'
import { useI18n } from 'vue-i18n'

export default defineComponent({
  name: 'VisualEditor',
  components: {
    VisualGauge, VisualFunnel, VisualRadar, VisualConfigure, VisualScatter,
    Card,
    Tooltip,
    RadioGroup, RadioGroupItem,
    ToggleGroup, ToggleGroupItem,
    Table,
    CircularLoading,
    Alert,
    Button,
    VisualWordCloud, VisualHistogram, VisualPie, VisualArea, VisualBar, VisualLine, VisualTable
  },
  computed: {
    Type()
    {
      return Type
    }
  },
  props: {
    loading: {
      type: Boolean,
      default: false
    },
    configuration: {
      type: Object as PropType<Configuration | null>
    }
  },
  setup()
  {
    const i18n = useI18n()

    return {
      i18n
    }
  },
  data()
  {
    return {
      configureVisible: false
    }
  },
  methods: {
    handlerCommit(value: any)
    {
      this.$emit('commitOptions', value)
    },
    forwardFiled(type: Type): ChartFieldGroup[]
    {
      return createdConfigure(type, this.i18n)
    }
  }
})
</script>
