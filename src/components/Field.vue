<template>
  <div class="container">
    <Lists v-bind:items="items" v-on:change-random="changeRandom" class="box" />
    <Results v-bind:items="items" v-on:change-random="changeRandom" class="box" />
  </div>
</template>

<script>
  import Lists from '@/components/Lists'
  import Results from '@/components/Results'

  export default {
    data() {
      return {
        items: [{
          id: 'List1',
          title: 'List 1',
          open: true,
          checked: false,
          indeterminate: true,
          sorted: true,
          randomColors: [],
          children: [{
            id: 'Item1',
            title: 'Item 1',
            color: '#e66465',
            checked: true,
            count: 20
          }, {
            id: 'Item2',
            title: 'Item 2',
            color: '#f6b73c',
            checked: false,
            count: 30
          }, {
            id: 'Item3',
            title: 'Item 3',
            color: '#ff0000',
            checked: false,
            count: 4
          }, {
            id: 'Item4',
            title: 'Item 4',
            color: '#ff00ff',
            checked: false,
            count: 0
          }]
        }, {
          id: 'List2',
          title: 'List 2',
          open: false,
          checked: false,
          indeterminate: false,
          sorted: true,
          randomColors: [],
          children: [{
            id: 'Item1',
            title: 'Item 1',
            color: '#e66465',
            checked: false,
            count: 20
          }, {
            id: 'Item2',
            title: 'Item 2',
            color: '#f6b73c',
            checked: false,
            count: 30
          }, {
            id: 'Item3',
            title: 'Item 3',
            color: '#ff0000',
            checked: false,
            count: 4
          }, {
            id: 'Item4',
            title: 'Item 4',
            color: '#ff00ff',
            checked: false,
            count: 0
          }]
        }, {
          id: 'List3',
          title: 'List 3',
          open: false,
          checked: false,
          indeterminate: false,
          sorted: true,
          randomColors: [],
          children: [{
            id: 'Item1',
            title: 'Item 1',
            color: '#e66465',
            checked: false,
            count: 20
          }, {
            id: 'Item2',
            title: 'Item 2',
            color: '#f6b73c',
            checked: false,
            count: 30
          }, {
            id: 'Item3',
            title: 'Item 3',
            color: '#ff0000',
            checked: false,
            count: 4
          }, {
            id: 'Item4',
            title: 'Item 4',
            color: '#ff00ff',
            checked: false,
            count: 0
          }]
        }, {
          id: 'List4',
          title: 'List 4',
          open: false,
          checked: false,
          indeterminate: false,
          sorted: true,
          randomColors: [],
          children: [{
            id: 'Item1',
            title: 'Item 1',
            color: '#e66465',
            checked: false,
            count: 20
          }, {
            id: 'Item2',
            title: 'Item 2',
            color: '#f6b73c',
            checked: false,
            count: 30
          }, {
            id: 'Item3',
            title: 'Item 3',
            color: '#ff0000',
            checked: false,
            count: 4
          }, {
            id: 'Item4',
            title: 'Item 4',
            color: '#ff00ff',
            checked: false,
            count: 0
          }]
        }, {
          id: 'List5',
          title: 'List 5',
          open: false,
          checked: false,
          indeterminate: false,
          sorted: true,
          randomColors: [],
          children: [{
            id: 'Item1',
            title: 'Item 1',
            color: '#e66465',
            checked: false,
            count: 20
          }, {
            id: 'Item2',
            title: 'Item 2',
            color: '#f6b73c',
            checked: false,
            count: 30
          }, {
            id: 'Item3',
            title: 'Item 3',
            color: '#ff0000',
            checked: false,
            count: 4
          }, {
            id: 'Item4',
            title: 'Item 4',
            color: '#ff00ff',
            checked: false,
            count: 0
          }]
        }]
      }
    },
    methods: {
      changeRandom(item) {
        const children = item.children.filter(el => el.checked);

        let resultArray = [];

        children.forEach(el => {
          if (Number(el.count)) {
            const y = `${el.color}/`.repeat(Number(el.count)).split('/');
            resultArray.push(...y);
            resultArray.length = resultArray.length - 1;
          }
        });

        for (let i = resultArray.length - 1; i > 0; i--) {
          let j = Math.floor(Math.random() * (i + 1));
          [resultArray[i], resultArray[j]] = [resultArray[j], resultArray[i]];
        }

        item.randomColors = resultArray;
      }
    },
    components: {
      Lists,
      Results,
    }
  }
</script>

<style>
  .container {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    padding: 10px;
    height: 100vh;
  }
  .box {
    border: 1px solid;
    padding: 20px;
    width: 45%;
    min-width: 300px;
    margin: 10px;
    flex-grow: 1;
  }
  .color-input {
    width: 20px;
    height: 20px;
    padding: 0;
    border: none;
    background: none;
    box-shadow: none;
    outline: none;
    cursor: pointer;
  }
  .color-input::-webkit-color-swatch-wrapper {
    padding: 0;
  }
  .color-input::-webkit-color-swatch {
    border: none;
    border-radius: 4px;
  }
</style>