<template>
<div class="component-table">
	<div class="table-header">
		<slot name="table-header">

		</slot>
    </div>
	<select-page-size
		v-if="!empty"
		:placeholder="page_size_placeholder"
		@changePageSize="changePageSize">
	</select-page-size>
	<search 
		v-if="!empty"
		:thead="tHead" 
		:search_placeholder="search_placeholder"
		:select_placeholder="select_placeholder"
		@onEnterSearch="onEnterSearch"
		>
	</search>
	<div class="dot" v-if="loadSearch">
		<div class="stage">
			<div class="dot-flashing"></div>
		</div>
	</div>
  	<table v-if="orderedList.length > 0" :class="{ 'load-table': loadSearch }">
        <thead>
			<tr>
				<th class="user-select" v-for="(item, x) in tHead" :key="x" @click="sortList(x)">
					<div class="thead">
						<div class="icon" v-if="x < tHead.length && item.typeof !== 'component' && item.typeof !== 'function'">
							<div class="arrow arr-up" :class="{ 'arrow-sort': item.isSort && !item.ascending }">
								<slot name="global-arrow-sort-icon-up">
									&#129169;
								</slot>
							</div>
							<div class="arrow arr-down" :class="{ 'arrow-sort': item.isSort && item.ascending }">
								<slot name="global-arrow-sort-icon-down">
									&#129171;
								</slot>
							</div>
						</div>
						<div>
							<slot :name="'header_column'" :item="item">
								<slot :name="'header_column_' + item.name" :item="item">
									{{ item.text }}
								</slot>
							</slot>
						</div>
					</div>
				</th>
				<th v-if="activeOperations">
					<div class="thead">
						<slot name="header_column_operations">
							عملیات
						</slot>
					</div>
				</th>
			</tr>
        </thead>
        <tbody>
			<tr class="row"  v-for="(item, index) in orderedList" :key="index">
				<!-- rows -->
				<td :data-th="item.name" v-for="(itemHead, indexHead) in tHead" :key="indexHead">
					<slot :name="'column_' + indexHead" :item="item" :i="itemHead">
						<slot v-if="itemHead.typeof === 'component'">
							<component v-bind="item.propsCopmonent" :is="item[itemHead.name]"  />
						</slot>
						<span v-else>
							{{ item[itemHead.name] }}
						</span>
					</slot>
				</td>
				<!-- operations -->
				<td data-th="operation" class="operation" v-if="activeOperations">
					<template v-for="(operate, x) in tOperations">
						<div v-if="operate.active" :key="x" :class="operate.class" class="item" @click="operation(operate,item)">
							<div>
								<slot :name="'operate_icon_' + operate.class" :img="operate.img">
									
								</slot>
							</div>
							<div>
								<slot :name="'operate_text_' + operate.class" :img="operate.img">
									{{ operate.text }}
								</slot>
							</div>
						</div>
					</template>
				</td>
			</tr>
        </tbody>
    </table>
	<div class="not-found" :class="{ 'load-table': loadSearch }" v-if="orderedList.length < 1 && !empty">
		<slot name="table-not-found">
				Not found
		</slot>
	</div>
	<div class="empty" v-if="orderedList.length < 1 && empty">
		<slot name="table-empty">
			Empty
		</slot>
	</div>
	<myPaginate
		:class="{ 'load-table': loadSearch }"
		v-if="totalPage>0"
        :current="currentPage"
        :total="tItems.length"
        :per-page="tPageSize"
        @page-changed="currentPageClick"
        text-before-input=" "
        text-after-input=" "/>
	<div class="table-footer">
		<slot name="table-footer">

		</slot>
	</div>
	<div id="wrapper" class="main">
		<div class="overlay" v-if="showModal" @click="showModal = false"></div>
		<div class="modal" v-if="showModal">
			<modal-dialog @Modalfalse="showModal = false">
				<header>
					<slot name="modal-header">
						<h3>اطلاعات</h3>
					</slot>
				</header>
				<main>
					<slot name="modal-main" :modalData="modalData">
						modal-main
					</slot>
				</main>
				<footer>
					<slot name="modal-footer">
						<button class="confirmation" @click="showModal = false">تایید</button>
					</slot>
				</footer>
			</modal-dialog>
		</div>
	</div>
</div> 
</template>

<script>
import MyPaginate from '@/components/my-paginate/my-paginate.vue'
import search from '@/components/search/search.vue'
import selectPageSize from '@/components/select/select-page-size.vue'
import ModalDialog from '@/components/modal-dialog/dialog.vue'

export default {
	name: 'sara-table',
	components: {
		MyPaginate,
		search,
		ModalDialog,
		selectPageSize
	},
	props: {
		items: {
			type: Array,
			default: () => ([])
		},
		search_placeholder: {
			type: String,
			default: 'جستجو'
		},
		select_placeholder: {
			type: String,
			default: 'سرچ بر اساس'
		},
		page_size_placeholder: {
			type: String,
			default: 'تعداد نمایش در هر صفحه'
		},
		tablehead: {
			type: Array,
			default: () => ([])
		},
		pageSize: {
			type: Number,
			default: 4
		},
		operations: {
			type: Array,
			default: [true, true]
		},
		modal: {
			type: Boolean,
			default: true
		}
	},
	data () {
		return {
			loadSearch: false,
			pageSort: '',
			pageSearch: '',
			disable: true,
			wordSearch: '',
			tHead: [],
			modalData: [],
			tItems: [],
			empty: false,
			currentPage: 1,
			tOperations: [],
			activeOperations: true,
			showUpto: 4,
			showModal: false,
			tPageSize: 3,
			showFromto: 0,
			wordSearch: '',
			headSearch: '',
			timeout: null
		}
	},
	mounted () {
		this.start()
	},
	computed: {
		orderedList () {
			const list = this.tItems.slice(this.showFromto, this.showUpto)
			return list
		},
		totalPage () {
			const total = Math.ceil(this.tItems.length / this.tPageSize)
			return total
		}
	},
	methods: {
		async setItems() {
			this.tItems = [...this.items]
			for(const items of this.tItems){
				for (const key in this.tHead){
					if(typeof items[this.tHead[key].name] === 'function' && this.tHead[key].typeof == 'function'){
						items[this.tHead[key].name] = await items[this.tHead[key].name]()
					}
				}
			}
		},
		async start() {
			this.tPageSize = this.pageSize
			this.showUpto = this.tPageSize
			this.tHead = [...this.tablehead]
			this.tHead.forEach((e) => {
				e.ascending = true
				e.isSort = false
			})
			if(!this.operations[0] && !this.operations[1]) {
				this.activeOperations = false
			}
			await this.setItems()
			if(this.tItems.length < 1) {
				this.empty = true
			}
			this.tOperations = [
				{
					text: 'حذف',
					class: 'delete',
					active: this.operations[0],
					func: function(self, item) {
						self.deleteItem(item)
					}
				},
				{
					text: 'مشاهده',
					class: 'showing',
					active: this.operations[1],
					func: function(self, item) {
						self.showItem(item)
					}
				}
			]
			// active Operations
			let count = this.operations.length
			this.operations.forEach((e) => {
				if (e === false){
					count--
				}
			})
			if(count==0){
				activeOperations=false
			}
		},
		changePageSize(pageSize) {
			this.tPageSize = pageSize
			this.showUpto = this.tPageSize
			this.currentPage = 1
			this.showFromto = 0
		},
		deleteItem(item) {
			this.tItems = this.tItems.filter(function(value){ 
				return value.id !== item.id;
			});
			this.$emit('deleteItem', item)
		},
		showItem(item) {
			if(this.modal) {
				this.modalData = item
				this.showModal = true
			}
			else this.$emit('showItem', item)
		},
		operation (i,item) {
			i.func(this,item)
		},
		sortList (i) {
			if(this.tHead[i].typeof !== 'component' && this.tHead[i].typeof !== 'function') {
				this.tHead.forEach((element, index) => {
				if (index === i) {
					element.ascending = !element.ascending
					element.isSort = true				
				} else {
					element.ascending = true
					element.isSort = false
				}
				})
				this.sort([i, this.tHead[i]])
			}
			
		},
		sort (i) {
			if (i[1].typeof === 'fa') {
				this.tItems.sort((a, b) => a[i[1].name].localeCompare(b[i[1].name], 'fa'))
			} else if(i[1].typeof === "number") {
				this.tItems.sort((a, b) => ( a[i[1].name] - b[i[1].name]));
			} else if (i[1].typeof === "date") {
				this.tItems.sort((a, b) => {
				const ca = +a[i[1].name].replace(/\//g, "");
				const cb = +b[i[1].name].replace(/\//g, "");
				return ca - cb;
			})
			} else {
				this.tItems.sort((a, b) => (a[i[1].name] > b[i[1].name]) ? 1 : ((b[i[1].name] > a[i[1].name]) ? -1 : 0))
			}
			if (i[1].ascending) {
				this.tItems.reverse()
			}
		},
		currentPageClick (i) {
			if(this.currentPage !== i) {
				this.currentPage = i
				this.showFromto = ((this.currentPage - 1) * this.tPageSize)
				this.showUpto = (this.currentPage * this.tPageSize)
			}
		},
		// search 
        isAll () {
            if (this.headSearch === '' || this.headSearch === '-1') {
                return true
            }
            return false
        },
		debounce (fn, delay) {
			var timeoutID = null
			return function () {
				clearTimeout(timeoutID)
				let args = arguments
				let that = this
				timeoutID = setTimeout(function () {
					fn.apply(that, args)
				}, delay)
			}
		},
        async onEnterSearch(e) {
			this.loadSearch = true
			if (this.timeout) clearTimeout(this.timeout)
			this.timeout = setTimeout(async () => {
				this.headSearch = e[0]
				this.wordSearch = e[1]
				this.tHead.forEach((element, index) => {
					element.ascending = true
					element.isSort = false
				})
				let items = []
				await this.setItems()
				if(this.wordSearch.length > 0){
					if (!this.isAll()) {
						for (const item of this.tItems) {
							const columenSearch = this.tHead[this.headSearch].name
							if(this.wordSearch.length > 0){
								const serachResult = item[columenSearch].includes(this.wordSearch)
								if (serachResult) {
									items.push(item)
								}
							} else {
								items.push(item)
							}
						}
					} else {
						for (const item of this.tItems) {
							for(let i=0; i<this.tHead.length; i++){
								const columenSearch = this.tHead[i].name
								if(item[columenSearch] && this.tHead[i].typeof !== 'component') {
									const serachResult = item[columenSearch].includes(this.wordSearch)
									if (serachResult) {
										items.push(item)
										break
									}
								}
							}
						}
					}
					this.tItems = items 
				} else await this.setItems()

				this.loadSearch = false
			}, 500)
			
        },
	}
}
</script>

<style scoped>
.not-found{
	width: 100%;
	text-align: center;
	font-size: 1.3rem;
}
.load-table{
	opacity: 0.7;
	filter: blur(.2em);
}
.stage {
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
    padding: 1rem 0;
    margin: 0 -5%;
    overflow: hidden;
}

.filter-contrast {
    filter: contrast(5);
    background-color: white;
}

.dot-flashing {
  position: relative;
  width: 10px;
  height: 10px;
  border-radius: 5px;
  background-color: #9880ff;
  color: #9880ff;
  animation: dotFlashing 1s infinite linear alternate;
  animation-delay: .5s;
}

.dot-flashing::before, .dot-flashing::after {
  content: '';
  display: inline-block;
  position: absolute;
  top: 0;
}

.dot-flashing::before {
  left: -15px;
  width: 10px;
  height: 10px;
  border-radius: 5px;
  background-color: #9880ff;
  color: #9880ff;
  animation: dotFlashing 1s infinite alternate;
  animation-delay: 0s;
}

.dot-flashing::after {
  left: 15px;
  width: 10px;
  height: 10px;
  border-radius: 5px;
  background-color: #9880ff;
  color: #9880ff;
  animation: dotFlashing 1s infinite alternate;
  animation-delay: 1s;
}

@keyframes dotFlashing {
  0% {
    background-color: #9880ff;
  }
  50%,
  100% {
    background-color: #ebe6ff;
  }
}
.overlay {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, .5);
}
.table-header{
	width: 100%;
	text-align: center;
	font-size: 33px;
}
.table-footer{
	width: 100%;
	text-align: center;
	font-size: 13px;
	margin: 20px 0;
}
section table {
	direction: rtl;
}
.thead p {
	margin: 0 4px 0 0;
}	

.component-table{
  direction: rtl;
}
.user-select {
	 -webkit-touch-callout: none;
	 -webkit-user-select: none;
	 -khtml-user-select: none;
	 -moz-user-select: none;
	 -ms-user-select: none;
	 user-select: none;
}
 select:required:invalid {
	 color: #666;
}
 .message {
	 width: 100%;
	 text-align: center;
}
.search-container {
	 width: 490px;
	 display: block;
	 margin: 0 auto;
}
 input#search-bar {
	 margin: 0 auto;
	 width: 100%;
	 height: 45px;
	 font-size: 1rem;
	 border: 1px solid #d0cfce 0;
	 outline: none;
}
 input#search-bar:focus {
	 border: 1px solid #008abf;
	 transition: 0.35s ease;
	 color: #008abf;
}
 input#search-bar:focus::-webkit-input-placeholder {
	 transition: opacity 0.45s ease;
	 opacity: 0;
}
 input#search-bar:focus::-moz-placeholder {
	 transition: opacity 0.45s ease;
	 opacity: 0;
}
 input#search-bar:focus:-ms-placeholder {
	 transition: opacity 0.45s ease;
	 opacity: 0;
}
 .search-icon {
	 position: absolute;
	 float: right;
	 width: 25px;
	 height: 25px;
	 top: 10px;
	 right: 20px;
}

 tr th {
	 cursor: pointer;
}
 tr th .thead {
	 display: flex;
	 flex-direction: row;
	 justify-content: center;
	 align-items: center;
}
 tr th .icon {
	 display: flex;
	 flex-direction: column;
	 margin: 0 10px;
	 width: 13px;
	 margin-top: -7px;
}
 tr th .icon .arrow {
	 display: flex;
	 font-size: 30px;
}
 tr th .icon .arrow-sort {
	 color: #a0a8d6;
	 filter: invert(69%) sepia(7%) saturate(1330%) hue-rotate(194deg) brightness(97%) contrast(90%);
}
 tr th .icon div {
	 color: #585d77;
	 filter: invert(34%) sepia(11%) saturate(1131%) hue-rotate(193deg) brightness(99%) contrast(83%);
}
 table {
	 border-collapse: separate;
	 border-spacing: 0 10px;
	 margin: 0;
	 padding: 0;
	 width: calc(100% - 45px);
	 table-layout: fixed;
	 font-size: 0.9rem;
}
 table tbody {
	 background-color: transparent;
}
 table tbody tr {
	 height: 80px;
	 margin: 20px 0px 10px 0px;
	 padding: 8px 20px 8px 40px;
	 border-radius: 15px;
	 color: #25265e;
	 box-shadow: -10px 10px 20px 0 rgba(37, 38, 94, 0.05);
	 background-color: transparent;
}
 table tbody tr td {
	 background-color: white;
}
 table tbody tr td:first-child {
	 border-radius: 0 15px 15px 0;
}
 table tbody tr td:last-child {
	 border-radius: 15px 0 0 15px;
}
 table tbody tr .operation {
	 display: flex;
	 flex-direction: row;
	 justify-content: space-evenly;
	 align-items: center;
	 height: 80px;
	 width: 90%;
	 padding: 0 5px 0 20px;
}
 table tbody tr .delete {
	 color: #ff6760;
	 filter: invert(51%) sepia(38%) saturate(990%) hue-rotate(314deg) brightness(100%) contrast(97%);
}
 table tbody tr .showing {
	 color: #6090f7;
	 filter: invert(54%) sepia(36%) saturate(2838%) hue-rotate(199deg) brightness(100%) contrast(94%);
}
 table tbody tr .edit img {
	 width: 25px;
}
 table tbody tr .item {
	 display: flex;
	 flex-direction: row;
	 align-items: center;
	 justify-content: space-around;
	 width: 30%;
	 cursor: pointer;
}
 table tbody tr .item div {
	 display: flex;
	 flex-direction: row;
	 align-items: center;
}
 table tbody tr .item div img {
	 width: 19px;
}
 table tbody tr .item:hover {
	 font-weight: bold;
}
 table caption {
	 font-size: 1.5em;
	 margin: 0.5em 0 0.75em;
}
 table tr {
	 background-color: transparent;
}
 table th, table td {
	 text-align: center;
}
 table th {
	 color: #878eb8;
	 font-size: 1.1rem;

}
 .select {
	 position: relative;
}
 .select select {
	 appearance: none;
	 outline: 0;
	 border: 0;
	 box-shadow: none;
	 background-image: none;
	 cursor: pointer;
}
 .select select::-ms-expand {
	 display: none;
}
 .select select {
	 width: 100%;
	 height: 50px;
	 padding: 10px 16px 11px 13px;
	 border-radius: 10px;
	 border: solid 1px rgba(19, 158, 202, 0.3);
	 background-color: #fff;
}
 .select::after {
	 position: absolute;
	 top: 11px;
	 left: 15px;
	 width: 15px;
	 background-color: transparent;
	 transition: 0.25s all ease;
	 pointer-events: none;
}
 .Pagesize {
	 width: 56%;
}
.main .modal {
	 position: absolute;
	 width: 40%;
	 z-index: 9999;
	 margin: 0 auto;
	 padding: 30px 30px 50px 30px;
	 background-color: #fff;
	 top: 30%;
	 left: 30%;
	 border-radius: 5px;
	 box-shadow: 0 4px 10px 0 rgba(40, 57, 79, 0.1);
	 border: solid 1px #bae3ef;
	 display: flex;
	 flex-direction: column;
}
 .main .modal footer {
	 display: flex;
	 justify-content: center;
	 align-items: center;
}
 .main .modal h3 {
	 font-size: 1.5rem;
	 font-weight: bold;
	 text-align: start;
}
 .main .modal .select {
	 position: relative;
}
 .main .modal .select select {
	 appearance: none;
	 outline: 0;
	 border: 0;
	 box-shadow: none;
	 background-image: none;
	 cursor: pointer;
}
 .main .modal .select select::-ms-expand {
	 display: none;
}
 .main .modal .select select {
	 width: 100%;
	 height: 50px;
	 padding: 10px 16px 11px 13px;
	 border-radius: 10px;
	 border: solid 1px rgba(19, 158, 202, 0.3);
	 background-color: #fff;
	 margin-bottom: 30px;
	 font-family: 'IRANYekan_n';
}
 .main .modal .select::after {
	 position: absolute;
	 top: 11px;
	 left: 15px;
	 background-color: transparent;
	 transition: 0.25s all ease;
	 pointer-events: none;
}
 .main .modal .confirmation {
	 width: 165px;
	 height: 50px;
	 flex-grow: 0;
	 text-align: center;
	 border-radius: 10px;
	 background-color: #30bfb7;
	 border: 0px;
	 opacity: 1;
	 align-self: end;
	 color: #fff;
	 font-family: 'IRANYekan_b';
	 font-size: 1.125rem;
	 cursor: pointer;
}
 .main .close {
	 background-color: transparent;
	 border: 0px;
	 font-size: 1.25rem;
	 align-self: end;
	 cursor: pointer;
}
 .main .modal {
	 direction: rtl;
	 position: absolute;
	 width: 40%;
	 text-align: start;
	 z-index: 9999;
	 margin: 0 auto;
	 padding: 30px 30px 50px 30px;
	 background-color: #fff;
	 top: 15%;
	 left: 30%;
	 border-radius: 5px;
	 box-shadow: 0 4px 10px 0 rgba(40, 57, 79, 0.1);
	 border: solid 1px #bae3ef;
	 background-color: #fafafa;
	 display: flex;
	 flex-direction: column;
}
 .main .modal h3 {
	 font-size: 1.5rem;
	 font-weight: bold;
	 text-align: center;
}
 .main .modal .select {
	 position: relative;
}
 .main .modal .select select {
	 appearance: none;
	 outline: 0;
	 border: 0;
	 box-shadow: none;
	 background-image: none;
	 cursor: pointer;
}
 .main .modal .select select::-ms-expand {
	 display: none;
}
 .main .modal .select select {
	 width: 100%;
	 height: 50px;
	 padding: 10px 16px 11px 13px;
	 border-radius: 10px;
	 border: solid 1px rgba(19, 158, 202, 0.3);
	 background-color: #fff;
	 margin-bottom: 30px;
	 font-family: 'IRANYekan_n';
}
 .main .modal .select::after {
	 position: absolute;
	 top: 11px;
	 left: 15px;
	 background-color: transparent;
	 transition: 0.25s all ease;
	 pointer-events: none;
}
 .main .modal .confirmation {
	 width: 165px;
	 height: 50px;
	 flex-grow: 0;
	 text-align: center;
	 border-radius: 10px;
	 background-color: #30bfb7;
	 border: 0px;
	 opacity: 1;
	 align-self: end;
	 color: #fff;
	 font-family: 'IRANYekan_b';
	 font-size: 1.125rem;
	 cursor: pointer;
}
 .main .close {
	 background-color: transparent;
	 border: 0px;
	 font-size: 1.25rem;
	 align-self: end;
	 cursor: pointer;
}
 
 @media only screen and (max-width: 500px) {
	 table {
		 border-collapse: separate;
		 border-spacing: 0 10px;
		 margin: 0;
		 padding: 0;
		 width: 700px;
	}
}
 @media only screen and (min-width: 500px) {
	 table {
		 border-collapse: separate;
		 border-spacing: 0 10px;
		 margin: 0;
		 padding: 0;
		 width: 700px;
	}
}
 @media only screen and (min-width: 798px) {
	 html {
		 font-size: 11px;
	}
	 table {
		 border-collapse: separate;
		 border-spacing: 0 10px;
		 margin: 0;
		 padding: 0;
		 width: 900px;
	}
}
 @media only screen and (min-width: 992px) {
	 html {
		 font-size: 12px;
	}
}
 @media only screen and (min-width: 1200px) {
	 html {
		 font-size: 14px;
	}
	 table {
		 border-collapse: separate;
		 border-spacing: 0 10px;
		 margin: 0;
		 padding: 0;
		 width: calc(100% - 0px);
	}
}
 @media only screen and (min-width: 1400px) {
	 html {
		 font-size: 16px;
	}
}
 
</style>