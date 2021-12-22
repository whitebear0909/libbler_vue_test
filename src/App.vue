<template>
  <div id="app">
    <div>
      <input class="num-input" type="text" v-model="inputStr" />
      <button class="btn-read" type="button" @click="readInput()">Read</button>
    </div>
    <div class="error-box" v-if="isEmpty">
      please fill the input field.
    </div>
    <div class="mt-20">
      <p class="display-box">
        {{displayStr}}
      </p>
    </div>
    <template v-for="phone in phones" >
      <div class="mt-10" :key="phone.id">
        <select
          @change="handleChange()"
          v-model="phone.prx"
          class="phone-select"
        >
          <option
            v-for="option in options"
            :value="option.id"
            :key="option.id"
          >
            {{ option.name }}
          </option>
        </select>
        <input class="phone-input" type="text" v-model="phone.content" @input="handleChange()"/>
        <button class="phone-close" type="button" @click="removePhone(phone.id)">X</button>
      </div>
    </template>
    <div class="mt-10">
      <select
          v-model="newPhone.prx"
          class="phone-select"
        >
          <option
            v-for="option in options"
            :value="option.id"
            :key="option.id"
          >
            {{ option.name }}
          </option>
        </select>
        <input class="phone-input" type="text" v-model="newPhone.content"/>
        <button class="phone-add" type="button" @click="addPhone()">+</button>
    </div>
    <div class="error-box" v-if="isNewPhoneEmpty">
      please fill the input field.
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  components: {
  },
  data: function() {
    return {
      inputStr: "",
      phones: [],
      options: [
        {id: "", name: ""},
        {id: "WA", name: "WA"},
        {id: "Home", name: "Home"},
        {id: "mobile", name: "mobile"},
      ],
      displayStr: "Phone number will be displayed here.",
      newPhone: {
        id: -1,
        prx: "",
        content: ""
      },
      isEmpty: false,
      isNewPhoneEmpty: false,
    };
  },
  methods: {
    readInput(){
      if(this.inputStr === ""){
        this.isEmpty = true;
        setTimeout(() => {
          this.isEmpty = false;
        }, 2000);
        return;
      }

      this.formatData();
      const strArray = this.inputStr.split(";");
      strArray.map((item, index) => {
        const phone = {};
        phone.id = index;
        const prxPos = item.indexOf(":");
        phone.prx = prxPos > 0 ? item.slice(0, prxPos) : "";
        phone.content = prxPos > 0 ? item.slice(prxPos+1, item.length) : item;
        this.phones.push(phone);
      });
      this.displayStr = this.inputStr;
      this.inputStr = "";
    },
    formatData(){
      this.phones = [];
      this.displayStr = "Phone number will be displayed here.";
    },
    removePhone(id){
      let temp = [];
      this.phones.map((item) => {
        if(item.id !== id) temp.push(item);
      });
      this.phones = temp;
      this.handleChange();
    },
    addPhone(){
      if(this.newPhone.content === ""){
        this.isNewPhoneEmpty = true;
        setTimeout(() => {
          this.isNewPhoneEmpty = false;
        }, 2000);
        return;
      }
      this.newPhone.id = this.phones.length;
      this.phones.push(this.newPhone);
      this.newPhone = {id: -1, prx: "", content: ""};
      this.handleChange();
    },
    handleChange(){
      this.displayStr = "";
      this.phones.map((item) => {
        let prxStr = item.prx === "" ? "" : item.prx + ": ";
        let phoneStr = prxStr + item.content + "; ";
        this.displayStr += phoneStr;
      });
      if(this.phones.length === 0) this.displayStr = "Phone number will be displayed here.";
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.num-input {
  border: 1px solid #81c7c4;
  padding: 10px 5px;
  border-radius: 5px 0px 0px 5px;
}

.num-input:focus {
  border: 1px solid #b0cecd;
  outline: none;
}

.btn-read {
  border-radius: 0px 5px 5px 0px;
  border: 1px solid #81c7c4;
  border-left: 0px !important;
  padding: 10px 5px;
  background-color: #81c7c4;
  color: white;
}

.btn-read:hover {
  cursor: pointer;
  background-color: #a0d9d6;
}

.mt-20 {
  margin-top: 20px;
}

.mt-10 {
  margin-top: 10px;
}

.display-box {
  padding: 5px 20px;
  border: 1px solid #81c7c4;
  background-color: cyan;
  display: inline-block;
  box-shadow: 2px 2px #c7eded;
}

.phone-select {
  border: 1px solid #81c7c4;
  padding: 6px 5px;
  border-radius: 5px 0px 0px 5px;
  min-height:20px;
  background-color: #81c7c4;
}

.phone-input {
  border: 1px solid #81c7c4;
  padding: 5px 5px;
  height: 20px;
  min-height: 20px;
}

.phone-input:focus {
  border: 1px solid #b0cecd;
  outline: none;
}

.phone-close {
  border: 1px solid #81c7c4;
  border-left: none !important;
  padding: 8px 10px;
  border-radius: 0px 5px 5px 0px;
  background-color: #d7a9c0;
}

.phone-close:hover {
  cursor: pointer;
  background-color: #ebc2d7;
}

.phone-add {
  border: 1px solid #81c7c4;
  border-left: none !important;
  padding: 8px 10px;
  border-radius: 0px 5px 5px 0px;
  background-color: #7285cd;
}

.phone-add:hover {
  cursor: pointer;
  background-color: #9aa9e3;
}

.error-box {
  color: red;
  margin-top: 5px;
}
</style>
