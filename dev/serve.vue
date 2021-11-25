
<template>
  <div id="app">
    <saragh-table-component-sample
    :tablehead="thead"
    :items="items"
    :modal="true"
    @showItem="showItem"
    @deleteItem="deleteItem"
    :operations="operations"
    :pageSize="pageSize"
      >
      <template slot="table-header">
        <h3> 
          <b>
            Table header
          </b>
        </h3>
      </template> 
      <template slot="table-footer">
        <i>
          Table footer
        </i>
      </template> 
      <!-- <template slot="global-arrow-sort-icon-up">
         
      </template>  -->
      <template slot="header_column" slot-scope="props">
          <!-- ** -->
          {{props.item.text}}
      </template>
      <!-- <template slot="header_column_fname" slot-scope="props">
          !!
          {{props.item.text}}
      </template> -->
      <!-- <template slot="column_4" >
          <btn :textBtn="'test'"></btn>
      </template> -->
      <template slot="modal-main" slot-scope="props">
        <div class="text">
          <div class="content">
            <p v-for="(item, x) in props.modalData" :key="x">
              <span>lable :</span>
              {{item}}
            </p>
          </div>
        </div>
      </template>
      <!-- <template slot="column_1" slot-scope="props">
          **
            {{props.item[props.i.name]}}
          **
      </template> -->
      <template slot="operate_icon_delete">
        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-trash" viewBox="0 0 16 16">
          <path d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0V6z"/>
          <path fill-rule="evenodd" d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1v1zM4.118 4 4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4H4.118zM2.5 3V2h11v1h-11z"/>
        </svg>
      </template>
      <template slot="operate_icon_showing">
        <img src="https://cdn1.iconfinder.com/data/icons/freeline/32/eye_preview_see_seen_view-256.png">
      </template>
    </saragh-table-component-sample>
  </div>
</template>

<style scoped>
#app{
  padding: 20px 3%;
}
</style>

<script>
import Vue from 'vue';
import btn from './btn.vue'

export default Vue.extend({
  name: 'ServeDev',
  components: {
   btn,
  },
  data () {
    return {
      items: [
          {
              id: "111fdfv",
              parent: "saraghotbzadeh",
              administratorName: "سارا قطب زاده",
              education: "vdfvdfv",
              jobSkills: "dfvdf",
              jobTitle: "fdvfdv",
              fname: "dfgvdfgd",
              lname: "fvdfvd",
              nationalCode: "dfdfv",
              phone: "09158854527",
              username: "fdfv" ,
              myfunction: ()=> 'klklk'
          },
          {
              id: "222fgbfgb",
              parent: "saraghotbzadeh",
              administratorName: "سارا قطب زاده",
              education: "fgbfgb",
              jobSkills: "bfgbfgb",
              jobTitle: "fgbfgb",
              fname: "fghfgg",
              lname: "bfgb",
              nationalCode: "gbfgbfg",
              phone: "09145556644",
              username: "fgbfgb",
              propsCopmonent: {
                textBtn: 12
              },
              mycomponent: () => import('./btn.vue')
          },
          {
              id: "333fgh",
              parent: "saraghotbzadeh",
              administratorName: "سارا قطب زاده",
              education: "بلابلا",
              jobSkills: "بلابلا",
              jobTitle: "بلابلا",
              fname: "فغغات",
              lname: "فغالبا",
              nationalCode: "بلابلا",
              phone: "09158889977",
              username: "بلابلا"
          },
          {
              id: "444dfdfv",
              parent: "saraghotbzadeh",
              administratorName: "سارا قطب زاده",
              education: "dfvdfv",
              jobSkills: "dfvdfv",
              jobTitle: "dfvdfv",
              fname: "dfgdfg",
              lname: "dfgdfv",
              nationalCode: "dfvdfv",
              phone: "09158845522",
              username: "dfdfv"
          },
          {
              id: "555gfhfghn",
              parent: "saraghotbzadeh",
              administratorName: "سارا قطب زاده",
              education: "fghfgh",
              jobSkills: "fghfgh",
              jobTitle: "fghfgh",
              fname: "rthglkmb",
              lname: "fgfghg",
              nationalCode: "fghfgh",
              phone: "09158975489",
              username: "gfhfghn"
          },
          {
              id: "666fghfg",
              parent: "saraghotbzadeh",
              administratorName: "سارا قطب زاده",
              education: "بلابلا",
              jobSkills: "بلابلا",
              jobTitle: "بلابلا",
              fname: "نتمت",
              lname: "سیبیت",
              nationalCode: "بلابلا",
              phone: "09158889977",
              username: "بلابلافغا"
          },
          {
              id: "777fdfv",
              parent: "saraghotbzadeh",
              administratorName: "سارا قطب زاده",
              education: "vdfvdfv",
              jobSkills: "dfvdf",
              jobTitle: "fdvfdv",
              fname: "dfgvdfgd",
              lname: "fvdfvd",
              nationalCode: "dfdfv",
              phone: "09158854527",
              username: "fdfv"
          },
          {
              id: "888fgbfgb",
              parent: "saraghotbzadeh",
              administratorName: "سارا قطب زاده",
              education: "fgbfgb",
              jobSkills: "bfgbfgb",
              jobTitle: "fgbfgb",
              fname: "fghfgg",
              lname: "bfgb",
              nationalCode: "gbfgbfg",
              phone: "09145556644",
              username: "fgbfgb"
          },
          {
              id: "999fghth",
              parent: "saraghotbzadeh",
              administratorName: "سارا قطب زاده",
              education: "بلابلا",
              jobSkills: "بلابلا",
              jobTitle: "بلابلا",
              fname: "فغغات",
              lname: "فغالبا",
              nationalCode: "بلابلا",
              phone: "09158889977",
              username: "بلابلا"
          },
          {
              id: "1010dfdfv",
              parent: "saraghotbzadeh",
              administratorName: "سارا قطب زاده",
              education: "dfvdfv",
              jobSkills: "dfvdfv",
              jobTitle: "dfvdfv",
              fname: "dfgdfg",
              lname: "dfgdfv",
              nationalCode: "dfvdfv",
              phone: "09158845522",
              username: "dfdfv"
          },
          {
              id: "1111gfhfghn",
              parent: "saraghotbzadeh",
              administratorName: "سارا قطب زاده",
              education: "fghfgh",
              jobSkills: "fghfgh",
              jobTitle: "fghfgh",
              fname: "rthglkmb",
              lname: "fgfghg",
              nationalCode: "fghfgh",
              phone: "09158975489",
              username: "gfhfghn"
          },
          {
              id: "1212sdegtr",
              parent: "saraghotbzadeh",
              administratorName: "سارا قطب زاده",
              education: "بلابلا",
              jobSkills: "بلابلا",
              jobTitle: "بلابلا",
              fname: "نتمت",
              lname: "سیبیت",
              nationalCode: "بلابلا",
              phone: "09158889977",
              username: "بلابلافغا"
          },
          {
              id: "1313fdfv",
              parent: "saraghotbzadeh",
              administratorName: "سارا قطب زاده",
              education: "vdfvdfv",
              jobSkills: "dfvdf",
              jobTitle: "fdvfdv",
              fname: "dfgvdfgd",
              lname: "fvdfvd",
              nationalCode: "dfdfv",
              phone: "09158854527",
              username: "fdfv"
          },
          {
              id: "1414fgbfgb",
              parent: "saraghotbzadeh",
              administratorName: "سارا قطب زاده",
              education: "fgbfgb",
              jobSkills: "bfgbfgb",
              jobTitle: "fgbfgb",
              fname: "fghfgg",
              lname: "bfgb",
              nationalCode: "gbfgbfg",
              phone: "09145556644",
              username: "fgbfgb"
          },
          {
              id: "1515fghyh",
              parent: "saraghotbzadeh",
              administratorName: "سارا قطب زاده",
              education: "بلابلا",
              jobSkills: "بلابلا",
              jobTitle: "بلابلا",
              fname: "فغغات",
              lname: "فغالبا",
              nationalCode: "بلابلا",
              phone: "09158889977",
              username: "بلابلا"
          },
          {
              id: "16dfdfv",
              parent: "saraghotbzadeh",
              administratorName: "سارا قطب زاده",
              education: "dfvdfv",
              jobSkills: "dfvdfv",
              jobTitle: "dfvdfv",
              fname: "dfgdfg",
              lname: "dfgdfv",
              nationalCode: "dfvdfv",
              phone: "09158845522",
              username: "dfdfv"
          },
          {
              id: "17gfhfghn",
              parent: "saraghotbzadeh",
              administratorName: "سارا قطب زاده",
              education: "fghfgh",
              jobSkills: "fghfgh",
              jobTitle: "fghfgh",
              fname: "rthglkmb",
              lname: "fgfghg",
              nationalCode: "fghfgh",
              phone: "09158975489",
              username: "gfhfghn"
          },
          {
              id: "18gytyu",
              parent: "saraghotbzadeh",
              administratorName: "سارا قطب زاده",
              education: "بلابلا",
              jobSkills: "بلابلا",
              jobTitle: "بلابلا",
              fname: "نتمت",
              lname: "سیبیت",
              nationalCode: "بلابلا",
              phone: "09158889977",
              username: "بلابلافغا"
          },
          {
              id: "19dfdfv",
              parent: "saraghotbzadeh",
              administratorName: "سارا قطب زاده",
              education: "dfvdfv",
              jobSkills: "dfvdfv",
              jobTitle: "dfvdfv",
              fname: "dfgdfg",
              lname: "dfgdfv",
              nationalCode: "dfvdfv",
              phone: "09158845522",
              username: "dfdfv"
          },
          {
              id: "20gfhfghn",
              parent: "saraghotbzadeh",
              administratorName: "سارا قطب زاده",
              education: "fghfgh",
              jobSkills: "fghfgh",
              jobTitle: "fghfgh",
              fname: "rthglkmb",
              lname: "fgfghg",
              nationalCode: "fghfgh",
              phone: "09158975489",
              username: "gfhfghn"
          },
          {
              id: "21gytyu",
              parent: "saraghotbzadeh",
              administratorName: "سارا قطب زاده",
              education: "بلابلا",
              jobSkills: "بلابلا",
              jobTitle: "بلابلا",
              fname: "نتمت",
              lname: "سیبیت",
              nationalCode: "بلابلا",
              phone: "09158889977",
              username: "بلابلافغا"
          },
          {
              id: "22dfdfv",
              parent: "saraghotbzadeh",
              administratorName: "سارا قطب زاده",
              education: "dfvdfv",
              jobSkills: "dfvdfv",
              jobTitle: "dfvdfv",
              fname: "dfgdfg",
              lname: "dfgdfv",
              nationalCode: "dfvdfv",
              phone: "09158845522",
              username: "dfdfv"
          },
          {
              id: "23gfhfghn",
              parent: "saraghotbzadeh",
              administratorName: "سارا قطب زاده",
              education: "fghfgh",
              jobSkills: "fghfgh",
              jobTitle: "fghfgh",
              fname: "rthglkmb",
              lname: "fgfghg",
              nationalCode: "fghfgh",
              phone: "09158975489",
              username: "gfhfghn"
          },
          {
              id: "24gytyu",
              parent: "saraghotbzadeh",
              administratorName: "سارا قطب زاده",
              education: "بلابلا",
              jobSkills: "بلابلا",
              jobTitle: "بلابلا",
              fname: "نتمت",
              lname: "سیبیت",
              nationalCode: "بلابلا",
              phone: "09158889977",
              username: "بلابلافغا"
          },
          {
              id: "25dfdfv",
              parent: "saraghotbzadeh",
              administratorName: "سارا قطب زاده",
              education: "dfvdfv",
              jobSkills: "dfvdfv",
              jobTitle: "dfvdfv",
              fname: "dfgdfg",
              lname: "dfgdfv",
              nationalCode: "dfvdfv",
              phone: "09158845522",
              username: "dfdfv"
          },
          {
              id: "26gfhfghn",
              parent: "saraghotbzadeh",
              administratorName: "سارا قطب زاده",
              education: "fghfgh",
              jobSkills: "fghfgh",
              jobTitle: "fghfgh",
              fname: "rthglkmb",
              lname: "fgfghg",
              nationalCode: "fghfgh",
              phone: "09158975489",
              username: "gfhfghn"
          },
          {
              id: "27gytyu",
              parent: "saraghotbzadeh",
              administratorName: "سارا قطب زاده",
              education: "بلابلا",
              jobSkills: "بلابلا",
              jobTitle: "بلابلا",
              fname: "نتمت",
              lname: "سیبیت",
              nationalCode: "بلابلا",
              phone: "09158889977",
              username: "بلابلافغا"
            
          }
      ],
      pageSize: 3,
      operations: [true, true],
      thead: [
        {
          text: 'نام',
          name: 'fname',
          typeof: 'fa'
        },
        {
          text: 'نام خانوادگی',
          name: 'lname',
          typeof: 'fa'
        },
        {
          text: 'نام کاربری',
          name: 'id',
          typeof: 'en'
        },
        {
          text: 'تلفن',
          name: 'phone',
          typeof: 'phone'
        },
        {
          text: 'component',
          name: 'mycomponent',
          typeof: 'component'
        },
        {
          text: 'function',
          name: 'myfunction',
          typeof: 'function'
        }
      ],
    }
  },
  methods: {
    deleteItem(item) {
      this.items = this.items.filter(function(value){ 
				return value.id !== item.id;
			});
      console.log('deleteItem');
    },
    showItem() {
      console.log('showItem');
    }
  }
});
</script>
