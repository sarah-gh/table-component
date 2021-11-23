<template>
    <div class="">
        <button class="close" @click="showModalfalse()"> 
			&#10006;
        </button>
        <header>
            <slot name="header"></slot>
        </header>
        <main>
            <slot></slot>
        </main>
        <footer>
            <slot name="footer"></slot>
        </footer>
    </div>
</template>

<script>
export default {
  name: 'modal-new-post',
  methods: {
    showModalfalse () {
      this.$emit('Modalfalse')
    }
  }

}
</script>

<style scoped>
.text {
	 text-align: center;
}
 .text h3 {
	 font-size: 1.5rem;
	 font-weight: bold;
	 color: #5864ff;
}
 .text .content {
	 width: 100%;
	 display: flex;
	 flex-direction: row;
	 flex-wrap: wrap;
	 justify-content: center;
	 align-items: center;
	 direction: rtl;
	 margin: 20px 0;
}
 .text .content p {
	 font-size: 1.2rem;
	 width: 50%;
	 margin: 20px 0;
	 text-align: start;
}
 .text .content .label {
	 color: gray;
	 padding-left: 10px;
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
 .modal {
	 position: absolute;
	 width: 555px;
	 z-index: 9999;
	 margin: 0 auto;
	 padding: 30px 30px 50px 30px;
	 background-color: #fff;
	 top: 40%;
	 left: 30%;
	 border-radius: 5px;
	 box-shadow: 0 4px 10px 0 rgba(40, 57, 79, 0.1);
	 border: solid 1px #bae3ef;
	 background-color: #fafafa;
	 display: flex;
	 flex-direction: column;
}
 .modal .confirmation {
	 width: 165px;
	 height: 50px;
	 flex-grow: 0;
	 text-align: center;
	 border-radius: 10px;
	 background-color: #5864ff;
	 border: 0px;
	 opacity: 1;
	 align-self: end;
	 color: #fff;
	 font-size: 1.2rem;
	 cursor: pointer;
}
 .close {
	 text-align: end;
	 background-color: transparent;
	 border: 0px;
	 width: 100%;
	 font-size: 1.4rem;
	 align-self: flex-end;
	 cursor: pointer;
}
 input[type="file"] {
	 display: none;
}
 
</style>
