<template>
  <div class="dropdown">
    <div class="buttons">
      <button @click="show = !show" class="drop">ستون ها {{show}} </button>
      <button @click="reset_url()" class="reset">حذف فیلتر ها</button>
    </div>
    <div class="parent">
      <ul ref="dropdown" v-show="show" :class="['dropdown-content', get_class]" :style="{'max-height': '900px', 'white-space': 'nowrap', 'display': 'block'}">
        <li v-for="item in all_columns" :key="item">
          <input type="checkbox" :checked="!unchecked_columns.includes(item)" :value="item" @change="value_changed(item, $event)" class="dropdown-checkbox">
          {{ item }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'col-selector',
  mounted(){

  },
  data: function(){
    return {
      show: true,
      dropdown_height: 0,
      top_offset: 0,
      unchecked_columns: [],
      initial_show: true,
    };
  },
  props: {
    all_columns: Array,
    table_height: Number,
    // show_dropdown: Boolean,
  },
  computed: {
    get_class(){
      return this.table_height - this.dropdown_height < 15 ?
        'down' :
        'up';
    },
  },
  methods:{
    value_changed(col_changed, event){
      event.stopPropagation();
      this.$emit('column_show_toggle', col_changed, event.target.checked);
    },
    reset_url(){
      this.$emit('reset_url');
    }
  },
  watch: {
    // unchecked_columns: function(newValue){
    //   // newValue.forEach(element => {
    //   //   // setTimeout(()=>{
    //   //   //   this.value_changed(element, false)
    //   //   // }, 0);
    //   // });
    // },
    table_height: function(){
      // if(!this.isMounted) return;
      // let d = //document.querySelector('.dropdown-content')
      let d = this.$refs.dropdown;
      this.dropdown_height = d.clientHeight < this.dropdown_height ? this.dropdown_height : d.clientHeight;
      this.top_offset = d.offsetTop;
      // this.show = false;
      if(this.initial_show) {
        this.$nextTick(() => {
          this.show = false;
          this.initial_show = false;
        })
      }
    }
  }
}
</script>

<style lang="scss" scoped>

  :host {
    overflow-y: visible;
  }

  .dropdown {
    .buttons {
      button {
        position: absolute;
        -moz-box-shadow:inset 0 1px 0 0 #94989c;
        -webkit-box-shadow:inset 0 1px 0 0 #94989c;
        box-shadow:inset 0 1px 0 0 #94989c;
        background:-webkit-gradient(linear, left top, left bottom, color-stop(0.05, #e0e0e0), color-stop(1, #cccccc));
        background:-moz-linear-gradient(top, #e0e0e0 5%, #cccccc 100%);
        background:-webkit-linear-gradient(top, #e0e0e0 5%, #cccccc 100%);
        background:-o-linear-gradient(top, #e0e0e0 5%, #cccccc 100%);
        background:-ms-linear-gradient(top, #e0e0e0 5%, #cccccc 100%);
        background:linear-gradient(to bottom, #e0e0e0 5%, #cccccc 100%);
        filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#e0e0e0', endColorstr='#cccccc',GradientType=0);
        background-color:#e0e0e0;
        -moz-border-radius:6px;
        -webkit-border-radius:6px;
        border-radius:6px;
        border:  1px solid grey;
        /* display:inline-block; */
        cursor:pointer;
        color:#5c5c5c;
        margin-left: 50px;
        /* padding:6px 24px; */
        text-decoration:none;
        /* text-shadow:0 1px 0 #696e73; */
        vertical-align: middle;
        height: 90%;
        margin-top: auto;
        margin-bottom: auto;
      }

      button:hover {
        background:-webkit-gradient(linear, left top, left bottom, color-stop(0.05, #cccccc), color-stop(1, #878787));
        background:-moz-linear-gradient(top, #cccccc 5%, #878787 100%);
        background:-webkit-linear-gradient(top, #cccccc 5%, #878787 100%);
        background:-o-linear-gradient(top, #cccccc 5%, #878787 100%);
        background:-ms-linear-gradient(top, #cccccc 5%, #878787 100%);
        background:linear-gradient(to bottom, #cccccc 5%, #878787 100%);
        filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#cccccc', endColorstr='#878787',GradientType=0);
        background-color:#cccccc;
      }
      button:active {
         position: absolute;
         top:0;
       }
      .drop {
        top: 1px;
        left: 0;
        width: 100px;
      }

      .reset {
        top:1px;
        left: 105px;
        width: 100px;
      }
    }

    .parent {
      .dropdown-content {
        display: block;
        position: absolute;
        border-radius: 10px;
        background-color: #f1f1f1;
        border: 1px solid gray;
        box-shadow: 4px 0 16px 4px rgba(0,0,0,0.2);
        z-index: 100;
        list-style: none;
        text-align: left;
        padding: 5px;

        &.up {
          bottom: -1em;
        }
        &.down {
          top: 0.5em;
        }

        .dropdown-checkbox {
          border-radius: 5px;
        }
      }

    }

  }

</style>
