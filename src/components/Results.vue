<template>
  <div>
    <div
      class="field" 
      v-for="item in items"
      :key="item.id">
      <p class="title">{{ item.title }}</p>
      <button v-if="item.sorted" v-on:click="getRandomColors(item)" class="btn">Перемешать</button>
      <button v-if="!item.sorted" v-on:click="item.sorted = !item.sorted" class="btn">Сортировать</button>

      <div class="colors" v-if="item.sorted">
        <div
          class="color-container"
          v-for="child in item.children.filter(i => i.checked)"
          :key="child.id">
            <input
              class="color-input"
              type="color"
              title="Удалить"
              v-bind:value="child.color"
              v-for="(color, index) in getColorsArray(child)"
              :key="index"
              v-on:click="deleteItem($event, item, child)"
              readonly>
          </div>
      </div>
      <div class="colors" v-if="!item.sorted">
        <input
          class="color-input"
          type="color"
          title="Удалить"
          v-for="(color, index) in item.randomColors"
          :key="index"
          v-bind:value="color"
          v-on:click="deleteItem($event, item, null, color, index)"
          readonly>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
      }
    },
    props: ['items'],
    methods: {
      getColorsArray(child) {
        let resultArray = [];

        resultArray.push(...`${child.color}/`.repeat(Number(child.count)).split('/'));
        resultArray.length = resultArray.length - 1;

        return resultArray;
      },
      getRandomColors(parent) {
        this.$emit('change-random', parent);
        
        parent.sorted = !parent.sorted;
      },
      deleteItem(event, parent, child, color, index) {
        event.preventDefault();

        const selectedChild = color ? parent.children.find(el => el.color === color) : child;

        selectedChild.count -= 1;

        
        if (typeof index === 'number') {
          parent.randomColors.splice(index, 1);
        }
      }
    }
  }
</script>

<style>
  .field {
    padding: 10px;
    margin-bottom: 10px;
    border: 1px solid;
    position: relative;
  }
  .btn {
    position: absolute;
    top: 10px;
    right: 10px;
    background: #69bf5f;
    border: none;
    outline: none;
    padding: 3px;
    border-radius: 3px;
    color: #fff;
    cursor: pointer;
  }
  .btn:hover {
    background: #509448;
  }
  .colors {
    margin-top: 10px;
  }
  .color-input {
    margin: 1px;
  }
</style>