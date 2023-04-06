<template>
  <div class="wholeContainer">
    
    <div id="addSection">
      <h1>Artist Directory</h1>
      <input type="text" v-model="filter" name="filter"></input>
      <button @click="showOptions" ref="addButton">Add artist</button>
    </div><br>
    
    <div id="categories" ref="categories" style="display:none;">

      <form @submit="onSubmit">
        <input type="text" v-model="artistName" name="artistName" placeholder="Artist Name"></input><br>
        <input type="text" v-model="aboutArtist" name="aboutArtist" placeholder="About artist"></input><br>
        <input type="text" v-model="imageUrl" placeholder="Image url"></input><br>
        <button type="submit" @click="onSubmit">Add</button>
      </form>

    </div>

    <div id="artists" ref="artists" style="display:none;">
      <div id="artistBox" ref="artistBox" v-model="artistBox" v-for="item in items" v-if="item.STATUS" >
        <div id="photo"><img :src="item.URL" :alt="item.NAME" width="50" height="50"></div>
        <div id="info">{{item.NAME}}<br>{{item.ABOUT}}</div>
        <div id="dbutton"><button @click="removeItems(item)">Delete</button></div>
      </div>
    </div>
  
  </div>
</template>


<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      status: true,
      artistBox: '',
      filter: '',
      artistName: '',
      aboutArtist: '',
      imageUrl: '',
      num: 0,
      items: [],
      products: []
    }
  },

  watch: {
    filter(newValue, oldValue) {
     
      this.hideBox(newValue);
      this.setAllStatusTrue();
      
    }
  },

  methods: {

    onInput() {
      this.hideBox(this.filter);
      this.setAllStatusTrue;
    },
    
    showOptions(event) {
      this.artistName = "";
      this.aboutArtist = "";
      this.imageUrl = "";
      this.$refs.artists.style.display="none";
      this.$refs.addButton.disabled = true;
      this.$refs.categories.style.display="block";
    },

    onSubmit(e) {
      e.preventDefault();
      this.$refs.categories.style.display="none";
      this.$refs.artists.style.display="block";
      console.log(this.artistName);
      console.log(this.aboutArtist);
      console.log(this.imageUrl);
      this.$refs.addButton.disabled = false;
      this.products = [{ 'NAME': this.artistName, 'ABOUT': this.aboutArtist, 'URL': this.imageUrl, 'STATUS': this.status }];
      this.addItems();
    }, 

    addItems() {
      for (var i = 0; i < this.products.length; i++)
      {
        let newItem = {
          'num': this.num,
          'NAME': this.artistName,
          'ABOUT': this.aboutArtist,
          'URL': this.imageUrl,
          'STATUS' : this.status
        };

        this.num += 1;
        this.items.push(newItem);
      }
    },

    setAllStatusTrue() {
      for (var i = 0; i < this.items.length; i++) {
        this.items.STATUS = true;
      }
    },

    removeItems(item) {
      for (var i = 0; i < this.items.length; i++)
      {
        if (this.items[i].num == item.num)
        {
          this.items.splice(i, 1); // remove 1 item at ith place
        }
      }
    },


    hideBox(item) {
      for(var i = 0; i < this.items.length; i++) {
        if(!this.isSub(item, this.items[i].NAME)) {
          this.items[i].STATUS = false;
        }
      }
    },

    isSub(item, name) {
      for(var i = 0; i < name.length; ++i) {
        for(var j = 0; j < item.length; ++j) {
          if(item[j].toLowerCase() != name[i].toLowerCase()) {
            return false;
          }
        }
        return true;
      }
    }


  }

}
</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

#artistBox {
  background-color: #ADD8E6;
  border: 1px solid white;
  border-radius: 20px;
  box-shadow: 6px 6px 6px rgb(29, 30, 31);
  width: 95vw;
  display: grid;
  place-items: center;
  grid-template-columns: 1fr 3fr 1fr;
  grid-template-areas:
  "photo info dbutton";
}

#photo {
  grid-area: photo;
}

#info {
  grid-area: info;
}

#dbutton {
  grid-area: dbutton;
}

img {
  border-radius: 50%;
}
</style>
