<template>
  <div class="min-w-max">
    <section class="flex justify-center bg-white rounded-lg border border-gray-200 px-10 py-3 text-gray-700 font-montserrat">
      <ul class="flex items-center">
        <li class="pr-6" v-if="hasPrev()">
          <a href="#" @click.prevent="changePage(prevPage)">
            <div class="flex items-center justify-center hover:bg-gray-200 rounded-md transform rotate-45 h-6 w-6">
              <div class="transform -rotate-45">
                <svg class="h-4 w-4" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"
                     stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7"/>
                </svg>
              </div>
            </div>
          </a>
        </li>
        <li class="pr-6" v-if="hasFirst()">
          <a href="#" @click.prevent="changePage(1)">
            <div class="flex hover:bg-gray-200 rounded-md transform rotate-45 h-6 w-6 items-center justify-center">
              <span class="transform -rotate-45">
                1
              </span>
            </div>
          </a>
        </li>
        <li class="pr-6" v-if="hasFirst()">...</li>
        <li class="pr-6" v-for="(page, x) in pages" :key="x">
          <a href="#" @click.prevent="changePage(page)">
            <div :class="{'bg-gradient-to-r from-blue-400 to-indigo-400': current == page}"
                 class="flex hover:bg-gray-200 rounded-md transform rotate-45 h-6 w-6 items-center justify-center">
              <span class="transform -rotate-45">{{ page }}</span>
            </div>
          </a>
        </li>
        <li class="pr-6" v-if="hasLast()">...</li>
        <li class="pr-6" v-if="hasLast()">
          <a href="#" @click.prevent="changePage(totalPages)">
            <div class="flex hover:bg-gray-200 rounded-md transform rotate-45 h-6 w-6 items-center justify-center">
          <span class="transform -rotate-45">
            {{ totalPages }}
          </span>
            </div>
          </a>
        </li>
        <li class="pr-6" v-if="hasNext()">
          <a href="#" @click.prevent="changePage(nextPage)">
            <div class="flex items-center justify-center hover:bg-gray-200 rounded-md transform rotate-45 h-6 w-6">
              <div class="transform -rotate-45">
                <svg class="h-4 w-4" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"
                     stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"/>
                </svg>
              </div>
            </div>
          </a>
        </li>
      </ul>
    </section>
  </div>
</template>

<script lang="ts">
export default {
  name: 'my-Pagination',
  props: {
    current: {
      type: Number,
      default: 1
    },
    total: {
      type: Number,
      default: 0
    },
    perPage: {
      type: Number,
      default: 9
    },
    pageRange: {
      type: Number,
      default: 2
    },
    textBeforeInput: {
      type: String,
      default: 'Go to page'
    },
    textAfterInput: {
      type: String,
      default: 'Go'
    }
  },
  data() {
    return {
      input: '',
    }
  },
  methods: {
    hasFirst: function () {
      return this.rangeStart !== 1
    },
    hasLast: function () {
      return this.rangeEnd < this.totalPages
    },
    hasPrev: function () {
      return this.current > 1
    },
    hasNext: function () {
      return this.current < this.totalPages
    },
    changePage: function (page) {
      if (page > 0 && page <= this.totalPages) {
        this.$emit('page-changed', page)
      }
    }
  },
  computed: {
    pages: function () {
      var pages = []

      for (var i = this.rangeStart; i <= this.rangeEnd; i++) {
        pages.push(i)
      }

      return pages
    },
    rangeStart: function () {
      var start = this.current - this.pageRange
      return (start > 0) ? start : 1
    },
    rangeEnd: function () {
      var end = this.current + this.pageRange
      return (end < this.totalPages) ? end : this.totalPages
    },
    totalPages: function () {
      return Math.ceil(this.total / this.perPage)
    },
    nextPage: function () {
      return this.current + 1
    },
    prevPage: function () {
      return this.current - 1
    }
  }
}
</script>

<style scoped>

hr {
    height: 0;
    color: inherit;
}
abbr[title] {
    -webkit-text-decoration: underline dotted;
    text-decoration: underline dotted;
}
b, strong {
    font-weight: bolder;
}
code, kbd, pre, samp {
    font-size: 1em;
}
small {
    font-size: 80%}
sub, sup {
    font-size: 75%;
    line-height: 0;
    position: relative;
    vertical-align: baseline;
}
sub {
    bottom: -.25em;
}
sup {
    top: -.5em;
}
table {
    text-indent: 0;
    border-color: inherit;
}
button, input, optgroup, select, textarea {
    font-size: 100%;
    line-height: 1.15;
    margin: 0;
}
button, select {
    text-transform: none;
}
[type=button], button {
    -webkit-appearance: button;
}
legend {
    padding: 0;
}
progress {
    vertical-align: baseline;
}
summary {
    display: list-item;
}
blockquote, dd, dl, figure, h1, h2, h3, h4, h5, h6, hr, p, pre {
    margin: 0;
}
button {
    background-color: transparent;
    background-image: none;
}
button:focus {
    outline: 1px dotted;
    outline: 5px auto -webkit-focus-ring-color;
}
fieldset {
    margin: 0;
    padding: 0;
}
ol, ul {
    list-style: none;
    margin: 0;
    padding: 0;
}
html {
    font-family: ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, "Noto Sans", sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji";
    line-height: 1.5;
}
body {
    font-family: inherit;
    line-height: inherit;
}
*, ::after, ::before {
    box-sizing: border-box;
    border-width: 0;
    border-style: solid;
    border-color: #e5e7eb;
}
hr {
    border-top-width: 1px;
}
img {
    border-style: solid;
}
textarea {
    resize: vertical;
}
input::-moz-placeholder, textarea::-moz-placeholder {
    opacity: 1;
    color: #9ca3af;
}
input:-ms-input-placeholder, textarea:-ms-input-placeholder {
    opacity: 1;
    color: #9ca3af;
}
input::placeholder, textarea::placeholder {
    opacity: 1;
    color: #9ca3af;
}
button {
    cursor: pointer;
}
table {
    border-collapse: collapse;
}
h1, h2, h3, h4, h5, h6 {
    font-size: inherit;
    font-weight: inherit;
}
a {
    color: inherit;
    text-decoration: inherit;
}
button, input, optgroup, select, textarea {
    padding: 0;
    line-height: inherit;
    color: inherit;
}
audio, canvas, embed, iframe, img, object, svg, video {
    display: block;
}
img, video {
    max-width: 100%;
    height: auto;
}
.bg-white {
    --tw-bg-opacity: 1;
    background-color: rgba(255, 255, 255, var(--tw-bg-opacity));
}
.bg-blue-50 {
    --tw-bg-opacity: 1;
    background-color: rgba(239, 246, 255, var(--tw-bg-opacity));
}
.hover\:bg-gray-200:hover {
    --tw-bg-opacity: 1;
    background-color: rgba(229, 231, 235, var(--tw-bg-opacity));
}
.bg-gradient-to-r {
    background-image: linear-gradient(to right, var(--tw-gradient-stops));
}
.from-blue-400 {
    --tw-gradient-from: #60a5fa;
    --tw-gradient-stops: var(--tw-gradient-from), var(--tw-gradient-to,  rgba(96,  165,  250,  0));
}
.to-indigo-400 {
    --tw-gradient-to: #818cf8;
}
.border-gray-200 {
    --tw-border-opacity: 1;
    border-color: rgba(229, 231, 235, var(--tw-border-opacity));
}
.border-indigo-400 {
    --tw-border-opacity: 1;
    border-color: rgba(129, 140, 248, var(--tw-border-opacity));
}
.rounded-md {
    border-radius: .375rem;
}
.rounded-lg {
    border-radius: .5rem;
}
.border {
    border-width: 1px;
}
.cursor-pointer {
    cursor: pointer;
}
.flex {
    display: flex;
}
.table {
    display: table;
}
.items-center {
    align-items: center;
    justify-content: center;
}
.justify-center {
    justify-content: center;
}
.justify-between {
    justify-content: space-between;
}
.font-medium {
    font-weight: 500;
}
.h-4 {
    height: 1rem;
}
.h-6 {
    height: 1.5rem;
}
.min-h-screen {
    min-height: 100vh;
}
.min-w-max {
    min-width: -webkit-max-content;
    min-width: -moz-max-content;
    min-width: max-content;
}
.focus\:outline-none:focus {
    outline: 2px solid transparent;
    outline-offset: 2px;
}
.p-5 {
    padding: 1.25rem;
}
.py-1 {
    padding-top: .25rem;
    padding-bottom: .25rem;
}
.px-1 {
    padding-left: .25rem;
    padding-right: .25rem;
}
.px-2 {
    padding-left: .5rem;
    padding-right: .5rem;
}
.py-3 {
    padding-top: .75rem;
    padding-bottom: .75rem;
}
.px-10 {
    padding-left: 2.5rem;
    padding-right: 2.5rem;
}
.pr-2 {
    padding-right: .5rem;
}
.pl-4 {
    padding-left: 1rem;
}
.pr-6 {
    padding-right: 1.5rem;
}
* {
    --tw-shadow: 0 0 #0000;
}
* {
    --tw-ring-inset: var(--tw-empty,  );
    /*!*//*!*/--tw-ring-offset-width: 0px;
    --tw-ring-offset-color: #fff;
    --tw-ring-color: rgba(59,  130,  246,  0.5);
    --tw-ring-offset-shadow: 0 0 #0000;
    --tw-ring-shadow: 0 0 #0000;
}
.text-gray-400 {
    --tw-text-opacity: 1;
    color: rgba(156, 163, 175, var(--tw-text-opacity));
}
.text-gray-700 {
    --tw-text-opacity: 1;
    color: rgba(55, 65, 81, var(--tw-text-opacity));
}
.w-4 {
    width: 1rem;
}
.w-6 {
    width: 1.5rem;
}
.w-12 {
    width: 3rem;
}
.w-14 {
    width: 3.5rem;
}
.transform {
    --tw-translate-x: 0;
    --tw-translate-y: 0;
    --tw-rotate: 0;
    --tw-skew-x: 0;
    --tw-skew-y: 0;
    --tw-scale-x: 1;
    --tw-scale-y: 1;
    transform: translateX(var(--tw-translate-x)) translateY(var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y));
}
.rotate-45 {
    --tw-rotate: 45deg;
}
.-rotate-45 {
    --tw-rotate: -45deg;
}
@-webkit-keyframes spin {
    to {
    transform: rotate(360deg);
}
}@keyframes spin {
    to {
    transform: rotate(360deg);
}
}@-webkit-keyframes ping {
    100%, 75% {
    transform: scale(2);
    opacity: 0;
}
}@keyframes ping {
    100%, 75% {
    transform: scale(2);
    opacity: 0;
}
}@-webkit-keyframes pulse {
    50% {
    opacity: .5;
}
}@keyframes pulse {
    50% {
    opacity: .5;
}
}@-webkit-keyframes bounce {
    0%, 100% {
    transform: translateY(-25%);
    -webkit-animation-timing-function: cubic-bezier(.8, 0, 1, 1);
    animation-timing-function: cubic-bezier(.8, 0, 1, 1);
}
50% {
    transform: none;
    -webkit-animation-timing-function: cubic-bezier(0, 0, .2, 1);
    animation-timing-function: cubic-bezier(0, 0, .2, 1);
}
}@keyframes bounce {
    0%, 100% {
    transform: translateY(-25%);
    -webkit-animation-timing-function: cubic-bezier(.8, 0, 1, 1);
    animation-timing-function: cubic-bezier(.8, 0, 1, 1);
}
50% {
    transform: none;
    -webkit-animation-timing-function: cubic-bezier(0, 0, .2, 1);
    animation-timing-function: cubic-bezier(0, 0, .2, 1);
}
}
ul {
    direction: ltr;
}
section{
    justify-content: center;
}
</style>