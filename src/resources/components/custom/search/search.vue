<template>
    <div>
        <div id="wrapper">
            <div class="table-form">
                <div class="search-container">
                    <div class="form-group select selectSearch">
                        <select v-model="headSearch" @change="searchSelect()" class="pageSearch">
                            <option value="" class="disabled-option" disabled selected hidden>{{ select_placeholder }}</option>
                            <option v-for="(item, x) in thead" :key="x" :value="x">{{ item.text }}</option>
                            <option value="-1" >همه</option>
                        </select>
                    </div>
                    <div class="form-group search">
                        <input type="search" id="search-bar" v-model="wordSearch" :placeholder="search_placeholder" v-on:keyup="onEnterSearch">
                        <img class="search-icon" src="https://img.icons8.com/ios/50/000000/search--v1.png">
                    </div>
                </div>
            </div>
        </div>
    </div>               
</template>

<script>
export default {
    name: 'search',
    props: {
        thead: {
            type: Array
        },
        items: {
            type: Array
        },
        search_placeholder: {
            type: String,
        },
        select_placeholder: {
            type: String,
        }
    },
    data() {
        return {
            wordSearch: '',
            headSearch: '',
            disable: true
        }
    },
    methods: {
        searchSelect () {
            this.disable = false
        },
        error () {
            if (this.headSearch === '' || this.headSearch === -1) {
                return false
            }
            return true
        },
        onEnterSearch(e) {
            this.$emit('onEnterSearch')
            if (this.error()) {
                for (const item of this.items) {
                    const includesSearchedWord = item[this.thead[this.headSearch].name].includes(this.wordSearch)
                    if (includesSearchedWord) {
                        this.addElement(item)
                    }
                }
            } else {
                let includesSearchedWord = ''
                for (const item of this.items) {
                    for(let i=0; i<this.thead.length; i++){
                        includesSearchedWord = item[this.thead[i].name].includes(this.wordSearch)
                        if (includesSearchedWord) {
                            this.addElement(item)
                            break
                        }
                    }
                }
            }
        },
        addElement (item) {
            this.$emit('addElement', item)
        },
    },
}
</script>

<style scoped>
.table {
	 height: 520px;
}
 header {
	 font-family: 'IRANYekan_b';
	 font-weight: bold;
	 font-stretch: normal;
	 font-style: normal;
	 line-height: 2;
	 letter-spacing: normal;
	 text-align: right;
	 color: #25265e;
}
 header h1 {
	 font-size: 2rem;
	 margin: 10px 0 30px 0;
}
 header p {
	 font-size: 1.2rem;
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
 input[type="file"] {
	 display: none;
}
 .content {
	 display: flex;
	 justify-content: space-between;
}
 .cropper-area {
	 width: 100%;
}
 body {
	 font: 18px/28px 'Noto Sans', serif;
	 padding: 0;
	 margin: 0;
}
 .pageSize {
	 margin: 0px 0 0 50px;
	 font: 18px/28px 'Noto Sans', serif;
	 padding: 5px 15px 5px 15px;
	 width: 382px;
	 display: block;
}
 #wrapper {
	 margin-top: 50px;
}
 #wrapper ::-webkit-scrollbar {
	 width: 5px;
	 height: 10px;
}
 #wrapper ::-webkit-scrollbar-track {
	 background-color: #fff;
	 border-radius: 10px;
}
 #wrapper ::-webkit-scrollbar-thumb {
	 background-color: #838383;
	 border-radius: 10px;
}
 .titlepageSize {
	 margin: 50px;
}
 select:required:invalid {
	 color: #666;
}
 .message {
	 width: 100%;
	 text-align: center;
}
 #datatable {
	 font-family: 'IRANYekan_b';
}
 .table-form {
	 margin: 30px 0;
}
 .table-form .search-container {
	 display: flex;
	 width: 100%;
	 justify-content: space-between;
	 align-items: center;
}
 .table-form .search-container .select {
	 width: 40%;
	 display: flex;
	 justify-content: flex-end;
}
 .table-form .search-container .selectSearch {
	 width: 15%;
	 display: flex;
	 justify-content: flex-end;
}
 .table-form .search-container .selectSearch::after {
	 content: '' !important;
}
 .table-form .search-container .selectSearch select::after {
	 content: '' !important;
}
 .table-form .search-container .selectSearch .pageSearch {
	 width: 100%;
	 border-radius: 15px;
	 box-shadow: -10px 10px 20px 0 rgba(37, 38, 94, 0.05);
	 background-color: white;
	 border: 0px;
	 height: 45px;
	 font-family: 'IRANYekan_b';
}
 .table-form .search-container .search {
	 width: 48%;
	 margin-right: -8%;
}
 .table-form .search-container .form-group {
	 position: relative;
}
 .table-form .search-container .form-group input {
	 padding: 0px 65px 0px 20px;
	 border-radius: 15px;
	 box-shadow: -10px 10px 20px 0 rgba(37, 38, 94, 0.05);
	 background-color: white;
	 border: 0px;
	 width: 80%;
	 font-family: 'IRANYekan_b';
}
 .table-form .search-container .form-group img {
	 width: 30px;
}
 .table-form .search-container .form-group select {
	 padding: 9px 25px 8px 20px;
	 border-radius: 8px;
	 border: 0px;
	 background-color: white;
	 width: 90%;
	 color: #7a7a86;
	 font-family: 'IRANYekan_b';
}
 .table-form .search-container .form-group select .disabled-option {
	 color: #c9c9da;
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
 .pager {
	 text-align: center;
	 width: 100%;
	 margin: 20px 0px 0px 15px;
	 display: flex;
	 flex-direction: row;
	 justify-content: center;
	 align-items: center;
}
 .pager .inactive {
	 color: gray;
	 width: 48px;
	 height: 48px;
	 margin: 0 35px;
	 border-radius: 10px;
	 box-shadow: 0 2px 4px 0 rgba(80, 80, 160, 0.16);
	 background-color: #eff4ff;
	 cursor: not-allowed;
}
 .pager .inactive img {
	 filter: invert(36%) sepia(68%) saturate(3240%) hue-rotate(225deg) brightness(101%) contrast(103%);
}
 .pager span {
	 font-weight: bold;
	 text-decoration: underline;
	 display: flex;
	 justify-content: center;
	 align-items: center;
	 cursor: pointer;
	 width: 48px;
	 height: 48px;
	 margin: 0 35px;
	 border-radius: 10px;
	 box-shadow: 0 2px 4px 0 rgba(80, 80, 160, 0.16);
	 background-color: #5864ff;
}
 .pager span img {
	 filter: brightness(6);
}
 .pager .pager-btn {
	 background-color: #fff;
	 display: flex;
	 border-radius: 25px;
	 flex-direction: row-reverse;
	 justify-content: center;
	 align-items: center;
}
 .pager button {
	 background-color: transparent;
	 border: 0;
	 width: 48px;
	 height: 48px;
}
 .pager .active {
	 padding: 11px 22px 12px;
	 border-radius: 10px;
	 box-shadow: 4px 4px 10px 0 rgba(80, 80, 160, 0.16);
	 background-color: #5864ff;
	 color: white;
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
}
 tr th .icon .arrow {
	 display: flex;
}
 tr th .icon div {
	 color: #a0a8d6;
	 filter: invert(69%) sepia(7%) saturate(1330%) hue-rotate(194deg) brightness(97%) contrast(90%);
}
 tr th .icon .arrow-sort {
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
	 font-family: 'IRANYekan_b';
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
	 justify-content: space-between;
	 align-items: center;
	 height: 80px;
	 width: 1205;
	 padding: 0 5px 0 20px;
}
 table tbody tr .delete {
	 color: #ff6760;
}
 table tbody tr .showing {
	 color: #6090f7;
}
 table tbody tr .edit img {
	 width: 25px;
}
 table tbody tr .item {
	 display: flex;
	 flex-direction: row;
	 align-items: center;
	 justify-content: space-around;
	 width: 33%;
	 cursor: pointer;
}
 table tbody tr .item div {
	 display: none;
	 flex-direction: row;
	 align-items: center;
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
	 font-size: 0.85em;
	 letter-spacing: 0.1em;
	 text-transform: uppercase;
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
	 font-family: 'IRANYekan_n';
}
 .select::after {
	 /* content: url("../assets/img/list/priority-low.svg"); */
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
 @media only screen and (max-width: 500px) {
	 .table {
		 overflow: auto;
	}
	 .table-form {
		 margin: 30px 0;
	}
	 .table-form .search-container {
		 display: flex;
		 width: 100%;
		 justify-content: space-between;
		 align-items: center;
		 flex-direction: column;
	}
	 .table-form .search-container .select {
		 width: 100%;
		 display: flex;
		 justify-content: flex-end;
	}
	 .table-form .search-container .selectSearch {
		 width: 100%;
		 display: flex;
		 justify-content: flex-end;
	}
	 .table-form .search-container .selectSearch .pageSearch {
		 width: 100%;
		 border-radius: 15px;
		 box-shadow: -10px 10px 20px 0 rgba(37, 38, 94, 0.05);
		 background-color: white;
		 border: 0px;
		 height: 45px;
		 font-family: 'IRANYekan_b';
	}
	 .table-form .search-container .search {
		 width: 100%;
		 margin-right: 0%;
	}
	 .table-form .search-container .form-group {
		 position: relative;
		 margin: 10px 0;
	}
	 .table-form .search-container .form-group input {
		 padding: 0px 65px 0px 20px;
		 border-radius: 15px;
		 box-shadow: -10px 10px 20px 0 rgba(37, 38, 94, 0.05);
		 background-color: white;
		 border: 0px;
		 width: 80%;
		 font-family: 'IRANYekan_b';
	}
	 .table-form .search-container .form-group img {
		 width: 30px;
	}
	 .table-form .search-container .form-group select {
		 padding: 9px 15px 8px 20px;
		 border-radius: 8px;
		 border: 0px;
		 background-color: white;
		 width: 100%;
		 color: #7a7a86;
		 font-size: 0.9rem;
		 font-family: 'IRANYekan_b';
	}
	 .table-form .search-container .form-group select .disabled-option {
		 color: #c9c9da;
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
}
 @media only screen and (min-width: 500px) {
	 .table {
		 overflow: auto;
	}
	 .table-form {
		 margin: 30px 0;
	}
	 .table-form .search-container {
		 display: flex;
		 width: 100%;
		 justify-content: space-between;
		 align-items: center;
		 flex-direction: column;
	}
	 .table-form .search-container .select {
		 width: 100%;
		 display: flex;
		 justify-content: flex-end;
	}
	 .table-form .search-container .selectSearch {
		 width: 100%;
		 display: flex;
		 justify-content: flex-end;
	}
	 .table-form .search-container .selectSearch .pageSearch {
		 width: 100%;
		 border-radius: 15px;
		 box-shadow: -10px 10px 20px 0 rgba(37, 38, 94, 0.05);
		 background-color: white;
		 border: 0px;
		 height: 45px;
		 font-family: 'IRANYekan_b';
	}
	 .table-form .search-container .search {
		 width: 100%;
		 margin-right: 0%;
	}
	 .table-form .search-container .form-group {
		 position: relative;
		 margin: 10px 0;
	}
	 .table-form .search-container .form-group input {
		 padding: 0px 65px 0px 20px;
		 border-radius: 15px;
		 box-shadow: -10px 10px 20px 0 rgba(37, 38, 94, 0.05);
		 background-color: white;
		 border: 0px;
		 width: 80%;
		 font-family: 'IRANYekan_b';
	}
	 .table-form .search-container .form-group img {
		 width: 30px;
	}
	 .table-form .search-container .form-group select {
		 padding: 9px 15px 8px 20px;
		 border-radius: 8px;
		 border: 0px;
		 background-color: white;
		 width: 100%;
		 color: #7a7a86;
		 font-size: 0.9rem;
		 font-family: 'IRANYekan_b';
	}
	 .table-form .search-container .form-group select .disabled-option {
		 color: #c9c9da;
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
}
 @media only screen and (min-width: 798px) {
	 .table-form {
		 margin: 30px 0;
	}
	 .table-form .search-container {
		 display: flex;
		 width: 100%;
		 justify-content: space-between;
		 align-items: center;
		 flex-direction: row;
	}
	 .table-form .search-container .select {
		 width: 40%;
		 display: flex;
		 justify-content: flex-end;
	}
	 .table-form .search-container .selectSearch {
		 width: 25%;
		 display: flex;
		 justify-content: flex-end;
	}
	 .table-form .search-container .selectSearch .pageSearch {
		 width: 100%;
		 border-radius: 15px;
		 box-shadow: -10px 10px 20px 0 rgba(37, 38, 94, 0.05);
		 background-color: white;
		 border: 0px;
		 height: 45px;
		 font-family: 'IRANYekan_b';
	}
	 .table-form .search-container .search {
		 width: 50%;
		 margin-right: -5% !important;
	}
	 .table-form .search-container .form-group {
		 position: relative;
		 margin: 0;
	}
	 .table-form .search-container .form-group input {
		 padding: 0px 65px 0px 20px;
		 border-radius: 15px;
		 box-shadow: -10px 10px 20px 0 rgba(37, 38, 94, 0.05);
		 background-color: white;
		 border: 0px;
		 width: 80%;
		 font-family: 'IRANYekan_b';
	}
	 .table-form .search-container .form-group img {
		 width: 30px;
	}
	 .table-form .search-container .form-group select {
		 padding: 9px 15px 8px 20px;
		 border-radius: 8px;
		 border: 0px;
		 background-color: white;
		 width: 90%;
		 color: #7a7a86;
		 font-size: 0.7rem;
		 font-family: 'IRANYekan_b';
	}
	 .table-form .search-container .form-group select .disabled-option {
		 color: #c9c9da;
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
}
 @media only screen and (min-width: 992px) {
	 .table-form {
		 margin: 30px 0;
	}
	 .table-form .search-container {
		 display: flex;
		 width: 100%;
		 justify-content: space-between;
		 align-items: center;
	}
	 .table-form .search-container .select {
		 width: 40%;
		 display: flex;
		 justify-content: flex-end;
	}
	 .table-form .search-container .selectSearch {
		 width: 15%;
		 display: flex;
		 justify-content: flex-end;
	}
	 .table-form .search-container .selectSearch .pageSearch {
		 width: 100%;
		 border-radius: 15px;
		 box-shadow: -10px 10px 20px 0 rgba(37, 38, 94, 0.05);
		 background-color: white;
		 border: 0px;
		 height: 45px;
		 font-family: 'IRANYekan_b';
	}
	 .table-form .search-container .search {
		 width: 48%;
		 margin-right: -8%;
	}
	 .table-form .search-container .form-group {
		 position: relative;
	}
	 .table-form .search-container .form-group input {
		 padding: 0px 65px 0px 20px;
		 border-radius: 15px;
		 box-shadow: -10px 10px 20px 0 rgba(37, 38, 94, 0.05);
		 background-color: white;
		 border: 0px;
		 width: 80%;
		 font-family: 'IRANYekan_b';
	}
	 .table-form .search-container .form-group img {
		 width: 30px;
	}
	 .table-form .search-container .form-group select {
		 padding: 9px 25px 8px 20px;
		 border-radius: 8px;
		 border: 0px;
		 background-color: white;
		 width: 90%;
		 color: #7a7a86;
		 font-size: 0.8rem;
		 font-family: 'IRANYekan_b';
	}
	 .table-form .search-container .form-group select .disabled-option {
		 color: #c9c9da;
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
}
 @media only screen and (min-width: 1200px) {
	 .table {
		 overflow: hidden;
	}
}
 @media only screen and (min-width: 1400px) {
	 html {
		 font-size: 16px;
	}
}
 
</style>