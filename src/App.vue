<template>
  <div id="app">
    <section class="filters">
      <div v-for="filter in filters">
        <Select
          :filter="filter"
          :chosenOptions="filter.chosenOptions"
          :isSingle="filter.isSingle"
          @select="selectOption"
          @clear="clearOption"
        />
      </div>
    </section>
  </div>
</template>

<script>
import Select from '@/components/Select.vue'

export default {
  name: 'App',
  components: {
    Select
  },
  data() {
    return {
      filters: [
        {
          isSingle: true,
          mainLabel: {
            label: 'Rok produkcji',
            value: 'rok_produkcji'
          },
          options: [
            {
              label: 'lorem ipsum lorem ipsum lorem',
              value: 'test'
            },
            {
              label: '2021',
              value: '2021'
            },
            {
              label: '2020',
              value: '2020'
            },
            {
              label: '2019',
              value: '2019'
            },
            {
              label: '2018',
              value: '2018'
            },
            {
              label: '2017',
              value: '2017'
            }
          ],
          chosenOptions: [
            {
              label: 'option 1',
              value: 'option1'
            }
          ]
        },
        {
          isSingle: true,
          mainLabel: {
            label: 'Moc',
            value: 'moc'
          },
          options: [
            {
              label: '50',
              value: '50'
            },
            {
              label: '60',
              value: '60'
            },
            {
              label: '70',
              value: '70'
            },
            {
              label: '80',
              value: '80'
            },
            {
              label: '90',
              value: '90'
            },
            {
              label: '100',
              value: '100'
            },
            {
              label: '110',
              value: '110'
            },
            {
              label: '120',
              value: '120'
            },
            {
              label: '130',
              value: '130'
            },
            {
              label: '140',
              value: '140'
            },
            {
              label: '150',
              value: '150'
            },
            {
              label: '160',
              value: '160'
            },
            {
              label: '170',
              value: '170'
            },
            {
              label: '180',
              value: '180'
            },
            {
              label: '190',
              value: '190'
            },
            {
              label: '200',
              value: '200'
            },
            {
              label: '210',
              value: '210'
            },
            {
              label: '220',
              value: '220'
            },
            {
              label: '230',
              value: '230'
            }
          ],
          chosenOptions: [
            {
              label: '220',
              value: '220'
            }
          ]
        },
        {
          mainLabel: {
            label: 'Placeholder',
            value: 'placeholder'
          },
          options: [
            {
              label: 'option 1',
              value: 'option1'
            },
            {
              label: 'option 2',
              value: 'option2'
            },
            {
              label: 'option 3',
              value: 'option3'
            },
            {
              label: 'option 4',
              value: 'option4'
            },
            {
              label: 'option 5',
              value: 'option5'
            },
            {
              label: 'option 6',
              value: 'option6'
            },
            {
              label: 'option 7',
              value: 'option7'
            },
            {
              label: 'option 8',
              value: 'option8'
            },
            {
              label: 'option 9',
              value: 'option9'
            },
            {
              label: 'option 10',
              value: 'option10'
            },
            {
              label: 'option 11',
              value: 'option11'
            },
            {
              label: 'option 12',
              value: 'option12'
            },
            {
              label: 'option 13',
              value: 'option13'
            },
            {
              label: 'option 14',
              value: 'option14'
            },
            {
              label: 'option 15',
              value: 'option15'
            },
            {
              label: 'option 16',
              value: 'option16'
            },
            {
              label: 'option 17',
              value: 'option17'
            },
            {
              label: 'option 18',
              value: 'option18'
            },
            {
              label: 'option 19',
              value: 'option19'
            }
          ],
          chosenOptions: [
            {
              label: 'option 1',
              value: 'option1'
            }
          ]
        }
      ]
    }
  },
  methods: {
    selectOption({ mainLabel, option: newOption, isSingle }) {
      const currentFilter = this.filters.find(
        item => item.mainLabel.value === mainLabel.value
      )

      const isNew = !currentFilter.chosenOptions.some(
        item => item.value === newOption.value
      )

      if (!isSingle) {
        return isNew
          ? (currentFilter.chosenOptions = [
              ...currentFilter.chosenOptions,
              newOption
            ])
          : (currentFilter.chosenOptions = currentFilter.chosenOptions.filter(
              item => item.value !== newOption.value
            ))
      }

      isNew
        ? (currentFilter.chosenOptions = [newOption])
        : (currentFilter.chosenOptions = [])
    },
    clearOption(mainLabel) {
      const currentFilter = this.filters.find(
        item => item.mainLabel.value === mainLabel.value
      )

      currentFilter.chosenOptions = []
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.filters {
  display: flex;
  gap: 15px;
}
</style>
