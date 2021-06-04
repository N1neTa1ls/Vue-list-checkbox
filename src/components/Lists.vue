<template>
  <div>
    <ul class="main-menu">
      <li
        v-for="item in items"
        :key="item.id"
        v-bind:class="{ open: item.open }"
      >
        <p class="title">
          <span v-on:click="item.open = !item.open" class="arrow">&#8250;</span>
          <label v-bind:class="{ indeterminate: item.indeterminate }">
            <input v-on:click="selectItems(item)" type="checkbox" class="checkbox" v-bind:checked="item.checked">
          </label>
          <span>{{ item.title }}</span>
        </p>

        <ul class="submenu">
          <li
            v-for="child in item.children"
            :key="child.title"
          >
            <p class="title">
              <input v-on:click="selectItem(child, item)" type="checkbox" class="checkbox" v-bind:checked="child.checked">
              <span>{{ child.title }}</span>
              <input class="count" type="number" max="100" min="0" v-model="child.count" v-on:change="changeRandom(item, child)">
              <input class="color-input" type="color" v-model="child.color" v-on:change="changeColor(item, child)">
            </p>
          </li>
        </ul>
      </li>
    </ul>
  </div>
</template>

<script>
  export default {
    data() {
      return {}
    },
    props: ['items'],
    methods: {
      selectItems(item) {
        if (item.checked) {
          item.children.forEach(child => {
            child.checked = false;
          });
        } else {
          item.children.forEach(child => {
            child.checked = true;
          });
        }

        if (!item.sorted) {
          this.$emit('change-random', item);
        }

        this.checkChildren(item);
      },
      selectItem(item, parent) {
        item.checked = !item.checked;
        if (!item.checked && !parent.sorted) {
          parent.randomColors = parent.randomColors.filter(el => el !== item.color);
        } else if (item.checked && !parent.sorted && Number(item.count)) {
          this.$emit('change-random', parent);
        }

        this.checkChildren(parent);
      },
      checkChildren(parent) {
        let allChildChecked = true;
        let hasOneCheked = false;

        parent.children.forEach(child => {
          if (!child.checked) {
            allChildChecked = false;
          } else {
            hasOneCheked = true;
          }
        });

        if (allChildChecked) {
          parent.checked = true;
          parent.indeterminate = false;
        } else if (hasOneCheked) {
          parent.checked = false;
          parent.indeterminate = true;
        } else {
          parent.checked = false;
          parent.indeterminate = false;
        }
      },
      changeRandom(item, child) {
        if (+child.count < 0) {
          child.count = 0;
        }
        if (child.checked) {
          this.$emit('change-random', item);
        }
      },
      changeColor(item, child) {
        const childrenColors = [];
        item.children.forEach(el => {
          childrenColors.push(el.color);
        });

        item.randomColors.forEach((el, index) => {
          if (childrenColors.indexOf(el) === -1) {
            item.randomColors[index] = child.color;
          }
        });

        item.randomColors = [...item.randomColors];
      },
    }
  }
</script>

<style>
  ul {
    list-style: none;
    padding: 0;
  }
  .title {
    display: flex;
    align-items: center;
  }
  .arrow {
    margin-right: 5px;
    padding: 5px;
    cursor: pointer;
  }
  .checkbox {
    margin: 5px;
    cursor: pointer;
  }
  .indeterminate {
    position: relative;
  }
  .indeterminate::before {
    display: block;
    content: '.';
    position: absolute;
    top: 8%;
    left: 48%;
    transform: translate(-50%, -50%);
    font-size: 30px;
  }
  .submenu {
    margin-left: 50px;
    height: 0;
    transition: all 0.3s ease;
    overflow: hidden;
  }
  .arrow {
    font-size: 20px;
    transform: rotate(0deg);
    transition: all 0.3s ease;
  }
  .main-menu .open .arrow {
    transform: rotate(90deg);
  }
  .main-menu .open .submenu {
    height: 100px;
  }
  .count {
    margin: 0 5px 0 auto;
    width: 40px;
    background: #eee;
    border: none;
    outline: none;
    text-align: right;
    padding: 2px 0;
  }
</style>