<template>
	<div class="table-form"> 
		<div class="form-group select select-search">
			<select v-model="headSearch" class="page-search">
				<option value="" class="disabled-option" disabled selected hidden>{{ select_placeholder }}</option>
				<template v-for="(item, x) in thead">
					<option :key="x" :value="x" v-if="item.typeof !== 'component' && item.typeof !== 'function'">
						{{ item.text }}
					</option>
				</template>
				<option value="-1" >همه</option>
			</select>
		</div>
		<div class="form-group search">
			<input type="search" id="search-bar" v-model="wordSearch" :placeholder="search_placeholder" v-on:keyup="onEnterSearch">
			<img class="search-icon" src="https://img.icons8.com/ios/50/000000/search--v1.png">
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
        onEnterSearch(e) {
            this.$emit('onEnterSearch' , [this.headSearch, this.wordSearch])
        },
    },
}
</script>

<style scoped>
 input[type="file"] {
	 display: none;
}

.table-form {
	margin: 80px 0 30px 0;
	display: flex;
    width: 100%;
    justify-content: center;
    align-items: center;
}
.table-form .select {
	 width: 40%;
	 display: flex;
	 justify-content: flex-end;
}
.table-form .select-search {
	 width: 15%;
	 display: flex;
	 justify-content: flex-end;
}
 .table-form .select-search::after {
	 content: '' !important;
}
 .table-form .select-search select::after {
	 content: '' !important;
}
 .table-form .select-search .page-search {
	 width: 100%;
	 border-radius: 15px;
	 box-shadow: -10px 10px 20px 0 rgba(37, 38, 94, 0.05);
	 background-color: white;
	 border: 0px;
	 height: 45px;
	 font-family: 'IRANYekan_b';
}
 .table-form .search {
	 width: 48%;
	 margin-right: -8%;
}
 .table-form .form-group {
	 position: relative;
}
 .table-form .form-group input {
	 padding: 0px 65px 0px 20px;
	 border-radius: 15px;
	 box-shadow: -10px 10px 20px 0 rgba(37, 38, 94, 0.05);
	 background-color: white;
	 border: 0px;
	 width: 80%;
	 font-family: 'IRANYekan_b';
}
 .table-form .form-group img {
	 width: 30px;
}
 .table-form .form-group select {
	 padding: 9px 25px 8px 20px;
	 border-radius: 8px;
	 border: 0px;
	 background-color: white;
	 width: 90%;
	 color: #7a7a86;
	 font-family: 'IRANYekan_b';
}
 .table-form .form-group select .disabled-option {
	 color: #c9c9da;
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
 @media only screen and (max-width: 500px) {
	 .table {
		 overflow: auto;
	}
	 .table-form {
		 margin: 30px 0;
	}
	 .table-form  {
		 display: flex;
		 width: 100%;
		 justify-content: space-between;
		 align-items: center;
		 flex-direction: column;
	}
	 .table-form  .select {
		 width: 100%;
		 display: flex;
		 justify-content: flex-end;
	}
	 .table-form  .select-search {
		 width: 100%;
		 display: flex;
		 justify-content: flex-end;
	}
	 .table-form  .select-search .page-search {
		 width: 100%;
		 border-radius: 15px;
		 box-shadow: -10px 10px 20px 0 rgba(37, 38, 94, 0.05);
		 background-color: white;
		 border: 0px;
		 height: 45px;
		 font-family: 'IRANYekan_b';
	}
	 .table-form  .search {
		 width: 100%;
		 margin-right: 0%;
	}
	 .table-form  .form-group {
		 position: relative;
		 margin: 10px 0;
	}
	 .table-form  .form-group input {
		 padding: 0px 65px 0px 20px;
		 border-radius: 15px;
		 box-shadow: -10px 10px 20px 0 rgba(37, 38, 94, 0.05);
		 background-color: white;
		 border: 0px;
		 width: 80%;
		 font-family: 'IRANYekan_b';
	}
	 .table-form  .form-group img {
		 width: 30px;
	}
	 .table-form  .form-group select {
		 padding: 9px 15px 8px 20px;
		 border-radius: 8px;
		 border: 0px;
		 background-color: white;
		 width: 100%;
		 color: #7a7a86;
		 font-size: 0.9rem;
		 font-family: 'IRANYekan_b';
	}
	 .table-form  .form-group select .disabled-option {
		 color: #c9c9da;
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
	 .table-form  {
		 display: flex;
		 width: 100%;
		 justify-content: space-between;
		 align-items: center;
		 flex-direction: column;
	}
	 .table-form  .select {
		 width: 100%;
		 display: flex;
		 justify-content: flex-end;
	}
	 .table-form  .select-search {
		 width: 100%;
		 display: flex;
		 justify-content: flex-end;
	}
	 .table-form  .select-search .page-search {
		 width: 100%;
		 border-radius: 15px;
		 box-shadow: -10px 10px 20px 0 rgba(37, 38, 94, 0.05);
		 background-color: white;
		 border: 0px;
		 height: 45px;
		 font-family: 'IRANYekan_b';
	}
	 .table-form  .search {
		 width: 100%;
		 margin-right: 0%;
	}
	 .table-form  .form-group {
		 position: relative;
		 margin: 10px 0;
	}
	 .table-form  .form-group input {
		 padding: 0px 65px 0px 20px;
		 border-radius: 15px;
		 box-shadow: -10px 10px 20px 0 rgba(37, 38, 94, 0.05);
		 background-color: white;
		 border: 0px;
		 width: 80%;
		 font-family: 'IRANYekan_b';
	}
	 .table-form  .form-group img {
		 width: 30px;
	}
	 .table-form  .form-group select {
		 padding: 9px 15px 8px 20px;
		 border-radius: 8px;
		 border: 0px;
		 background-color: white;
		 width: 100%;
		 color: #7a7a86;
		 font-size: 0.9rem;
		 font-family: 'IRANYekan_b';
	}
	 .table-form  .form-group select .disabled-option {
		 color: #c9c9da;
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
	 .table-form  {
		 display: flex;
		 width: 100%;
		 justify-content: center;
		 align-items: center;
		 flex-direction: row;
	}
	 .table-form  .select {
		 width: 40%;
		 display: flex;
		 justify-content: flex-end;
	}
	 .table-form  .select-search {
		 width: 25%;
		 display: flex;
		 justify-content: flex-end;
	}
	 .table-form  .select-search .page-search {
		 width: 100%;
		 border-radius: 15px;
		 box-shadow: -10px 10px 20px 0 rgba(37, 38, 94, 0.05);
		 background-color: white;
		 border: 0px;
		 height: 45px;
		 font-family: 'IRANYekan_b';
	}
	 .table-form  .search {
		 width: 50%;
		 margin-right: -5% !important;
	}
	 .table-form  .form-group {
		 position: relative;
		 margin: 0;
	}
	 .table-form  .form-group input {
		 padding: 0px 65px 0px 20px;
		 border-radius: 15px;
		 box-shadow: -10px 10px 20px 0 rgba(37, 38, 94, 0.05);
		 background-color: white;
		 border: 0px;
		 width: 80%;
		 font-family: 'IRANYekan_b';
	}
	 .table-form  .form-group img {
		 width: 30px;
	}
	 .table-form  .form-group select {
		 padding: 9px 15px 8px 20px;
		 border-radius: 8px;
		 border: 0px;
		 background-color: white;
		 width: 90%;
		 color: #7a7a86;
		 font-size: 0.7rem;
		 font-family: 'IRANYekan_b';
	}
	 .table-form  .form-group select .disabled-option {
		 color: #c9c9da;
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
	 .table-form  {
		 display: flex;
		 width: 100%;
		 justify-content: center;
		 align-items: center;
	}
	 .table-form  .select {
		 width: 40%;
		 display: flex;
		 justify-content: flex-end;
	}
	 .table-form  .select-search {
		 width: 15%;
		 display: flex;
		 justify-content: flex-end;
	}
	 .table-form .select-search .page-search {
		 width: 100%;
		 border-radius: 15px;
		 box-shadow: -10px 10px 20px 0 rgba(37, 38, 94, 0.05);
		 background-color: white;
		 border: 0px;
		 height: 45px;
		 font-family: 'IRANYekan_b';
	}
	 .table-form .search {
		 width: 48%;
		 margin-right: -8%;
	}
	 .table-form .form-group {
		 position: relative;
	}
	 .table-form .form-group input {
		 padding: 0px 65px 0px 20px;
		 border-radius: 15px;
		 box-shadow: -10px 10px 20px 0 rgba(37, 38, 94, 0.05);
		 background-color: white;
		 border: 0px;
		 width: 80%;
		 font-family: 'IRANYekan_b';
	}
	 .table-form .form-group img {
		 width: 30px;
	}
	 .table-form .form-group select {
		 padding: 9px 25px 8px 20px;
		 border-radius: 8px;
		 border: 0px;
		 background-color: white;
		 width: 90%;
		 color: #7a7a86;
		 font-size: 0.8rem;
		 font-family: 'IRANYekan_b';
	}
	 .table-form .form-group select .disabled-option {
		 color: #c9c9da;
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
 
</style>