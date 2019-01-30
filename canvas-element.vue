<template>
  <div :class="'v-canvas-container '  + flipping" :id="name" @click="flipMe(this)">
  <div :class="'flipper v-canvas-box w-elevation-' + elevation" :style="{ background: color , width: width  , height: height}">
    <div class="v-canvas-box-card front" :style="{height:height}">

      <div v-if="icon && !image" :class="' wicon  w-elevation-' + elevation" :style="{ background: icon_box }">
        <i class="material-icons" :style="{color:icon_color}">{{icon}}</i>
      </div>
      <div v-if="image" :class="'wimg w-elevation-' + elevation" :style="'background:url(' + image + ')'">

      </div>
      <div class="v-canvas-box-title" :style="{'border-bottom': divider + 'px solid ' + text_color }">

          <div class="box-label" :style="{color:text_color}" v-html="label"></div>
          <div class="box-title" :style="{color:text_color}" v-html="title"></div>
          <div class="box-content" :style="{color:text_color}" v-html="content"></div>

      </div>
      <div class="v-canvas-box-footer" :style="{color:text_color}">
        <div>
          <button v-if="!flip" v-html="slot_1" :style="{color:text_color}"  @click="click(name,1)"></button>
        </div>
        <div>
          <button v-if="!flip" v-html="slot_2" :style="{color:text_color}" @click="click(name,2)"></button>
          <button v-if="flip" v-html="flip_text" :style="{color:text_color}"></button>
        </div>
        <div>
          <button v-if="!flip" v-html="slot_3" :style="{color:text_color}" @click="click(name,3)"></button>
        </div>
      </div>
    </div>

    <div v-if="flip" class="v-canvas-box-card back" :style="{background: back_color, height: height}">
      <div class="v-canvas-back-content v-canvas-box-title" :style="{'border-bottom': divider + 'px solid ' + text_color }" v-html="back_content" ></div>
      <div class="v-canvas-box-footer" :style="{color:text_color}">
        <div>
          <button v-html="slot_1" :style="{color:text_color}"  @click="click(name,1)"></button>
        </div>
        <div>
          <button v-html="slot_2" :style="{color:text_color}" @click="click(name,2)"></button>
        </div>
        <div>
          <button v-html="slot_3" :style="{color:text_color}" @click="click(name,3)"></button>
        </div>
      </div>
    </div>

  </div>
</div>
</template>
<script>
export default {
  data:()=>({
    size: '',
    flipping: '',
    divide: '',
    degree:'',
  }),
  props:{
    name: { type: String , required: false , default: '' },
    width: { type: String, required: false , default: '19rem' },
    height: { type: String, required: false , default: '9.5rem' },
    title: { type: String, required:false , default: 'TITLE' },
    label: { type: String, required:false , default: 'Label' },
    content: { type: String, required: false , default: 'abstract'},
    elevation: { type: String, required:false , default: '1' },
    color: { type: String, required:false , default: '#fff' },
    text_color: { type: String, required:false , default: '#555' },
    border: { type: String, required:false , default: '0' },
    icon_box: { type: String, required:false , default: '#fff' },
    icon_color: { type: String, required:false , default: '#555' },
    image: { type: String, required:false , default: '' },
    icon: { type: String, required:false , default: 'business' },
    slot_1: { type: String, required: false , default: ''},
    slot_2: { type: String, required: false , default: ''},
    slot_3: { type: String , required: false , default: '' },
    back_content: { type: String, required: false, default: 'back content' },
    back_color: { type: String , required: false , default: '#fff' },
    flip: { type: Boolean , required: false , default: false },
    flip_text : { type: String , required: false, default : '<i class="material-icons">keyboard_arrow_down</i>' },
    divider: { type: String, required: false , default: '0'}
  },

  methods:{
    click(id,slot){
      this.$emit('slotClick',id,slot)
    },
    flipMe(){
      if ( document.getElementById(this.name) && this.flip ){
        let flipdiv = document.getElementById(this.name)
        if ( !flipdiv.classList.contains('flipped') ){
          flipdiv.classList.add('flipping','flipped');
        } else {
          flipdiv.classList.remove('flipped');
        }
      }
    },
  },
  watch:{
    flip(val){
      val ? this.flipping = 'flipping' : this.flipping = ''
    }
  },
  mounted(){
    this.divider ? this.divide = 'style="border-bottom: ' + this.divider + 'px solid ' + this.text_color +  '"': this.divide = ''
    if ( this.flip ){
      if ( this.name ){
        this.flip ? this.flipping = 'flipping' : this.flipping = ''
        var flipdiv = document.getElementById(this.name)
      }
    }
  }
}
</script>
<style>

.v-canvas-container {
  margin-bottom:1rem;
}

.flipping .back {
    -webkit-transform: rotateY( 180deg );
    -moz-transform: rotateY( 180deg );
    -o-transform: rotateY( 180deg );
    transform: rotateY( 180deg );
    transition: 0.6s;
    transform-style: preserve-3d;
    position: relative;
}

.flipping.flipped {
    -webkit-transform: rotateY( 180deg );
    -moz-transform: rotateY( 180deg );
    -o-transform: rotateY( 180deg );
    transform: rotateY( 180deg );
    transition: 0.6s;
  	transform-style: preserve-3d;
  	position: relative;
}

/* flip the pane when hovered */
.flipping:hover .flipper, .flipping.hover .flipper {
  /*transform: rotateY(180deg);*/
}

.flipped {
  transform: rotateY(180deg);
}

.flipping {
  transform: rotateY(0deg);
  transition: 0.6s;
  transform-style: preserve-3d;
  position: relative;
}

/* flip speed goes here */
.flipper {
	transition: 0.6s;
	transform-style: preserve-3d;
	position: relative;
}

/* hide back of pane during swap */
.front, .back {
	backface-visibility: hidden;
	position: absolute;
	top: 0;
	left: 0;
}

/* front pane, placed above back */
.front {
	z-index: 2;
	/* for firefox 31 */
	transform: rotateY(0deg);
}

/* back, initially hidden pane */
.back {
	transform: rotateY(180deg);
  border-radius:.5rem;
  height:100%;
  background-size: cover!important;
}

/* flip the pane when hovered */
.v-canvas-box:hover .flipper, .v-canvas-box.hover .flipper {
	transform: rotateY(180deg);
}

.widget-container {
  max-width: 100%;
  display: grid;
  justify-content: space-evenly;
  grid-gap: 0.5rem;
  grid-template-columns: repeat(auto-fill, 320px);
  margin-bottom: 0rem;
}
.v-canvas-box {
  perspective: 1000px;
  display: block;
  justify-content: space-evenly;
  min-height: 9.5rem;
  height: 9.5rem;
  min-width: 19rem;
  width: 19rem;
  margin-bottom: 0rem;
  border-radius:.5rem;
  margin:0 auto;
  background-size: cover!important;
}

.v-canvas-box a { text-decoration:none; color:unset; }

.v-canvas-box-card {
  display: block;
  border: 0px solid #eaeaea;
  width: 100%;
  margin-bottom:1rem;
  margin:0 auto;
  background-size: cover!important;
}

.widget-back-card {
  position: relative;
  /*top: -10.5rem;*/
  width: 98%;
  margin: 0 auto;
  min-height: 9rem;
  border-radius: .5rem;
}

.v-canvas-box-card > .wicon {
  position: absolute;
  margin: .6rem 0 0 .6rem;
  border-radius: 0.2rem;
  padding: .7rem .7rem .7rem .7rem;
}

.v-canvas-box-card > .wimg {
  position: absolute;
  margin: 1rem 0 0 1rem;
  border-radius: 0.2rem;
  min-height: 4.5rem;
  min-width: 4.5rem;
  background-size: cover !important;
  background-position: center center !important;
  background-repeat: no-repeat !important;
}

.wicon i {
  font-size:3rem;
}


.v-canvas-box-title {
  min-height: 5.5rem;
  max-height: 5.5rem;
  padding: 1rem;
  text-align: right;
  border-bottom:0px solid #dfdfdf;
}

.v-canvas-box-footer {
  position: absolute;
  width:100%;
  bottom: 0;
  max-width: 100%;
  margin-top: .3rem;
  min-height: 1rem;
  padding-bottom: .5rem;
  display: grid;
  justify-content: space-evenly;
  grid-gap: 0.002rem;
  grid-template-columns: 1fr 3fr 1fr;
  align-items:center;
}


.v-canvas-box-footer > div > button {
  background: transparent;
  border: 0;
  font-size:.7rem;
  height: calc(width*100)
}


.v-canvas-box-footer > div > button > .material-icons { font-size:1.2rem; }

.w-elevation-1 {
  box-shadow: 0 3px 1px -2px rgba(0, 0, 0, 0.2), 0 2px 2px 0 rgba(0, 0, 0, 0.14), 0 1px 5px 0 rgba(0, 0, 0, 0.12) !important;
}
.w-elevation-2 {
  box-shadow: 0px 3px 5px -1px rgba(0,0,0,0.2), 0px 5px 8px 0px rgba(0,0,0,0.14), 0px 1px 14px 0px rgba(0,0,0,0.12) !important;
}

.w-elevation-3 {
  box-shadow: 0px 5px 5px -3px rgba(0,0,0,0.2), 0px 8px 10px 1px rgba(0,0,0,0.14), 0px 3px 14px 2px rgba(0,0,0,0.12) !important;
}

.box-title {
  font-size: 1.6rem;
  font-weight:bold;
  margin-left:30%;
  line-height:1.3rem;
}
.box-label {
  font-size: .7rem;
}
.box-content {
  font-size: .6rem;
  opacity:.8;
  margin-left:4rem;
}
.v-canvas-box-border-1 {
  border-bottom: 1px solid #eaeaea;
}

.v-canvas-back-content {
  margin:.2rem auto;
  text-align:left;
}

</style>
