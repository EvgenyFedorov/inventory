<template>
  <div id="my-list" v-on:mousemove="moveAt" ref="parentDiv">
    <div v-for="(row, rowKey) in setting.rows" ref="div"
        :key="rowKey">
      <div v-for="(column, columnKey) in setting.columns"
           v-bind:key="columnKey"
           v-bind:ref="'columnKey' + getItemKey(columnKey, rowKey)"
           v-bind:style="{
                position: positionByKey(columnKey),
                left: leftByKey(columnKey),
                top: topByKey(columnKey),
                width: setting.widthColumn + 'px',
                height: setting.heightColumn + 'px'
           }"
           v-bind:id="getGeneratedIdColumn(columnKey, rowKey)"
           v-bind:content="getItem(columnKey + rowKey + '0')"
           v-on:mousedown="myDrag(columnKey, $event)"
           v-on:mouseup="myDragStop">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'UlList',
  props: {
    msg: String,
    inventory: Object
  },
  data () {
    return {
      setting: this.inventory.setting,
      items: this.inventory.items,
      column_index: 1,
      column_num: 1
    }
  },
  mounted() {
    this.formInventory();
  },
  methods: {
    formInventory(){
      let columns = this.$refs;
      //let columns_count = this.setting.rows * this.setting.columns;

      let iter_num = 0;

      for(let item in columns){

        if(item !== 'div') {

          iter_num++;

          let good_item = columns[item][0];

          this.items.forEach((inventoryItem) => {

            if(iter_num === inventoryItem.key){

              //console.log(inventoryItem.title + "|" + iter_num + "|" + inventoryItem.key + "+");
              good_item.innerHTML = inventoryItem.title;

            }else{

              //console.log(inventoryItem.title + "|" + iter_num + "|" + inventoryItem.key);
              good_item.innerHTML = iter_num;

            }

          });

        }
      }
    },
    getItemKey(columnKey, rowKey){
      if(columnKey === 0 && rowKey === 0){
        this.column_index = 0;
      }
      let col_key = columnKey;
      columnKey = col_key+columnKey;
      return this.column_index = (this.column_index + 1);
    },
    getGeneratedIdColumn(columnKey, rowKey){
      if(columnKey === 0 && rowKey === 0){
        this.column_num = 0;
      }
      let col_key = columnKey;
      columnKey = col_key+columnKey;
      return this.column_num = (this.column_num + 1);
    },
    getItem(){




      // this.$refs.dev.forEach(function (elem, key){
      //
      //   console.log(elem);
      //   console.log(key);
      //
      //   elem.forEach((item, key) => {
      //     console.log(key);
      //   });
      //
      // });

    //getItem(rowKey, columnKey){
      //let column_id = rowKey + '' + '' + columnKey

      //this.$refs.column[column_id].innerText = rowKey + '' + '' + columnKey;
      //return '';

      // let ref_id = rowKey + '' + '' + columnKey;
      //
      // this.items.forEach(() => {
      //   console.log(ref_id);
      //   console.log(this.$refs.div);
      //    //return (item.key == rowKey + '' + '' + columnKey) ? item.title : item.key + ' | ' + rowKey + '' + '' + columnKey;
      // });

      // console.log(rowKey, columnKey);
      // for(let i=0; i <= this.items.length; i++){
      //   console.log(i);
      //   var local_item = this.items[i];
      //   return (local_item.key == rowKey + '' + '' + columnKey) ? local_item.title : local_item.key + ' | ' + rowKey + '' + '' + columnKey;
      // }

    },
    myDrag(key, event) {
      this.draggableIndex = key;
      this.reorderedList = this.items;
      this.moveAt(event);
    },
    moveAt(event){
      if(this.draggableIndex !== -1){
        let reorderedList=[];
        let lastI = -1;
        // получить отображаемые элементы списка
        // и их координаты

        this.$refs.li.forEach((elem, i) => {
          // игнорируем перетаскиваемый элемент
          if (i !== this.draggableIndex) {
            if (elem.offsetTop < event.pageY
                && i > this.draggableIndex) {
              // если мы здесь, то элемент переместился вверх
              reorderedList[i - 1] = this.items[i];
              if (lastI === -1 || lastI < i) {
                lastI = i;
              }
            } else if (elem.offsetTop > event.pageY
                && i < this.draggableIndex) {
              // если мы здесь, то элемент переместился вниз

              reorderedList[i + 1] = this.items[i];
              if (lastI === -1 || lastI > i) {
                lastI = i;
              }
            } else {
              //иначе его позиции не изменились
              reorderedList[i] = this.items[i];
            }
          }
        });
        // если позиции изменились, мы должны переопределить элементы
        if (lastI !== -1) {
          reorderedList[lastI] = this.items[this.draggableIndex];
          this.reorderedList = Object.assign([], reorderedList, reorderedList);
        }
        this.dragLeft = event.pageX - 16;
        this.dragTop = event.pageY - 16;
      }
    },
    myDragStop() {
      this.draggableIndex = -1;
      this.dragLeft = 0;
      this.dragTop = 0;

      this.items = Object.assign([], this.reorderedList, this.reorderedList);
    },
    isDragging(key) {
      return this.draggableIndex === key;
    },
    positionByKey(key) {
      return this.isDragging(key) ? 'absolute' : 'relative'
    },
    leftByKey(key) {
      return this.isDragging(key) ? this.dragLeft + 'px' : 0
    },
    topByKey(key) {
      return this.isDragging(key) ? this.dragTop + 'px' : 0
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
#my-list {
  display: block;
  margin: 0 10px;
}
#my-list div {
  display: block;
  vertical-align: middle;
  text-align: center;
}
#my-list div div{
  display: inline-block;
  border: 1px solid #ff0000;
}
a {
  color: #42b983;
}
</style>
