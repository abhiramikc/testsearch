<template>
    
    <h1>Search images</h1>
    <form id="search-form" @submit.prevent="onSearch" >
    <input type="text" v-model="searchTerm" v-on:keyup.enter="onSearch" placeholder="Type to search.."
     class="input is-medium">
    <button type="submit">Search</button>
    </form>
    
    <!-- <div v-for="d in data" :key="d.id" class="masonary"> -->


<div v-if="isSearching">
<vueper-slides autoplay fade fixed-height="500px"> >
  <vueper-slide v-for="(slide,i) in data" :key="i" :image="slide.urls.small" @click="generatePdf(slide)"
  :content="slide.alt_description"/>  
</vueper-slides>
</div>


<!-- 
      <div class="item">
        <img :src="`${d.urls.small}`" @mouseover="mouseOver" >
      </div> -->
        
<!--     
 <div v-show="active">
<p>{{d.alt_description}}</p>
<p>{{d.description}}</p>
 </div> -->
   
    <!-- </div> -->


<div>







</div>
</template>
<script>
import axios from "axios"
import jspdf from "jspdf";
import { VueperSlides, VueperSlide } from 'vueperslides';
import 'vueperslides/dist/vueperslides.css';
export default{
  components:{
    // eslint-disable-next-line vue/no-unused-components
    VueperSlides,VueperSlide
  },
data(){
    return{
searchTerm:'',
drinks:{},
data:{},
active:false,
isSearching: false,
logo:null,

    }
},
mounted(){
    
        // Reveal.initialize();


console.log("mounted");
},
methods:{
  // getDataUri(imgUrl, function(dataUri){
  //   this.logo=dataUri;
  // });

 getDataUri(url)
{

    //return new Promise(resolve => {
        var image = new Image();
        image.setAttribute('crossOrigin', 'anonymous'); //getting images from external domain

        image.onload = function () {
            var canvas = document.createElement('canvas');
            canvas.width = this.naturalWidth;
            canvas.height = this.naturalHeight; 

            //next three lines for white background in case png has a transparent background
            var ctx = canvas.getContext('2d');
            ctx.fillStyle = '#fff';  /// set white fill style
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            canvas.getContext('2d').drawImage(this, 0, 0);

            // eslint-disable-next-line no-undef
            resolve(canvas.toDataURL('image/jpeg'));
        };

        image.src = url;
        return image;
   // })
}
,


  generatePdf(img)
  {


    const doc=new jspdf();
    let left = 25;
    let top = 16;
    const imgWidth = 100;
    const imgHeight = 100;
    var logo = this.getDataUri(img.urls.small);
    doc.addImage(logo, 'jpeg', left, top, imgWidth, imgHeight);
    doc.text(`bio is ${img.user.bio}`, 8, 8);
    doc.text(`portfolio url is : ${img.user.portfolio_url}`,12,12)

    doc.save("test.pdf");
  },


onSearch()
{
    
    let clientId="GC-KvTTPZn0Wp4njM5IY9YelcWtsqdg9Q5fWNRJRdEI";
    let searchT=this.searchTerm;
    let url1=`https://api.unsplash.com/search/photos?page=1&query=${searchT}&client_id=${clientId}`;
    axios.get(url1).then((response) => {
    console.log("response get-axios",response.data.results);
    this.data=response.data.results;
    this.isSearching=true;

   
})
},
        mouseOver: function(){
            this.active = !this.active;   
        }
},

}
</script>

<style>

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 100vh;
}
button {
  background-color: #0780a8; /* Green */
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
}




#search-form  {
  padding-left: 20%;
  padding-right: 20%;
  background: #fefdff;
  min-height: 10px;

  height: 10vh;
  display: flex;
  align-items: center;   
  transition: 1s cubic-bezier(.7,.28,.47,1.15) height;  

 
}
input {
  margin-right: 5px;
  width: 50%;
  padding: 12px 20px;
  margin: 8px 0;
  box-sizing: border-box;
  }



.vueperslide {
  background: linear-gradient(-45deg, #EE7752, #E73C7E, #23A6D5, #23D5AB);

  &__title {
    font-size: 7em;
    opacity: 0.6;
  }
}

// documentation link.
p {
  position: absolute;
  top: 5px;
  right: 5px;
  z-index: 10;
}

html {font: 12px Tahoma, Geneva, sans-serif;}
* {margin: 0;padding: 0;color: rgb(15, 14, 14);}

.vueperslides--fixed-height { 
  height: 100%; 
  width: 100%; 
  }

</style>

