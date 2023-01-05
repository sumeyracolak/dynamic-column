<template>
  <div id="divCreatedRow" v-if="isSubmitted">
    <div class="container">
      <div class="row" v-for="(row,i) in rows" :key="i" draggable="true" @dragstart="dragStart(i,$event)" @dragover.prevent @dragend="dragEnd" @drop="dragFinish(i,$event)"> 
        <div :class="'col col-' + cols.width + ''" v-for="cols in row.kolon">
          <span>+</span>
        </div>
        <div class="row-actions">
          <div class="close-row-btn" @click="removeRow(i)">x</div>
        </div>
      </div>
    </div>
  </div>
  <div id="divSelectRow">
    <div class="add-col-div" v-if="show">
      <div class="add-col-btn" @click="show = !show">+</div>
      <div class="add-col-text">Yeni Satır Ekle</div>
    </div>
    <div class="select-col-div" v-else>
      <h3>SATIR YAPISINI SEÇİNİZ</h3>
      <ul class="columns">
        <li class="column" v-for="data in SectionData">
          <svg viewBox="0 0 100 50" @click="addRow(data)">
            <path :d=[data.svgattr] />
          </svg>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  import SectionData from './sectiondata.json'
  export default{
    data(){
      return{
        show:true,
        isSubmitted : false,
        SectionData,
        rows: [],
        dragging: -1
      }
    },
    methods:{
      addRow(data){
        this.show=!this.show;
        this.isSubmitted = true;

        this.rows.push({id: data.id, kolon: data.kolon})
      },
      removeRow(i){
        this.rows.splice(i, 1);
      },
      dragStart(which, ev) {
        ev.dataTransfer.setData('text/plain', this.id);
        ev.dataTransfer.dropEffect = 'move'
        this.dragging = which;
      },
      dragFinish(to, ev) {
        this.moveItem(this.dragging, to);
      },
      moveItem(from, to) {
        if (to === -1) {
          this.removeItemAt(from);
        } else {
          this.rows.splice(to, 0, this.rows.splice(from, 1)[0]);
        }
      },
      dragEnd(ev) {
        this.dragging = -1
      }
    },
    computed: {
      isDragging() {
        return this.dragging > -1
      }
    }
  }
</script>

<style>
  body{font-family:'Roboto',sans-serif!important;}

  #divCreatedRow{float:left;width:100%;padding:50px;}
  #divSelectRow{width: 70%;padding: 50px; margin: 50px auto 0; border: 2px dashed #ddd;position: relative;display: flex;justify-content: center;align-items: center;flex-direction: column;text-align: center;}
  .close-btn{position: absolute;right: 0; top: 0;width: 30px;height: 30px;line-height: 30px; cursor: pointer; font-size: 20px;}
  #divSelectRow .columns{display: flex;flex-wrap: wrap;}
  #divSelectRow .column{flex: calc(100% / 6);}
  #divSelectRow .column svg{float:left;fill:#ddd;cursor:pointer;}
  #divSelectRow .column svg:hover{fill:#000;}

  .add-col-div{display: flex;justify-content: center;align-items: center;flex-direction: column;}
  .add-col-btn{width: 50px; height: 50px; border-radius: 50%;background: blue; color: #fff;line-height: 50px; cursor: pointer;font-size: 30px;}
  .add-col-text{margin-top: 10px;}

  .select-col-div{float:left;width:100%;}
  .select-col-div h3{margin-bottom: 20px;}

  .row{display: flex;margin: 0 -15px; padding: 0;position: relative;}
  .col{padding: 15px;}
  .col-100{width: 100%}
  .col-66{width: 66.6666%}
  .col-50{width: 50%}
  .col-33{width: 33.3333%}
  .col-25{width: 25%}
  .col-20{width: 20%}
  .col-16{width: 16.6666%}
  .col > span{display:block;border:2px dashed #000;text-align:center;line-height:30px;font-size:30px;color:#999;cursor:pointer;}

  .row-actions{position:absolute;left:50%;transform:translateX(-50%);top:-21px;display:none;padding:0 20px;line-height:20px;text-align:center;}
  .close-row-btn{display: inline-block;vertical-align: middle;color: #fff;cursor: pointer;}
  .row-actions:before{content:'';position:absolute;border-bottom:20px solid lightblue;border-left:15px solid transparent;border-right:15px solid transparent;width:100%;z-index:-1;left: 0;}
  #divCreatedRow .row:hover .row-actions{display: block;}
  #divCreatedRow .row:hover{outline:2px solid lightblue;}

</style>