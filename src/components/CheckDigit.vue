<template>
  <div class="hello">
    <h1>Check Valid ISBN-13</h1>
    <div class="news">
      <marquee>
        The messages will be removed automatically, in case if any left try focus out and then focus into the input
      </marquee>
    </div>
    <div class="check-digit-inputs" id="check-digit-inputs">
      <input
          v-model="isbnInput"
          type="text"
          name="check-digit-input"
          id="check-digit-input"
          @keyup.enter="checkISBN"
          @focus="clearMessages"
      />
      <input
          ref="button"
          type="button"
          value="->"
          @click="checkISBN"
          id="check-digit-button"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: "CheckDigit",
  data() {
    return {
      isbnInput: "",
    };
  },
  methods: {
    checkISBN() {
      this.$refs.button.focus();
      const dataArray = this.isbnInput.split("");
      if (dataArray.length === 12) {
        this.findLast(dataArray);
      } else if (dataArray.length === 13) {
        this.checkValidISBN13(dataArray);
      } else {
        this.printIt("Not a valid ISBN-13 number", "error");
      }
    },
    findLast(data) {
      let sum = 0;
      for (let index = 0; index < data.length; index++) {
        index % 2 === 0 ? (sum += data[index] * 1) : (sum += data[index] * 3);
      }
      const mod = sum % 10;
      data.push(10 - mod);
      this.checkValidISBN13(data);
    },
    printIt(value, className) {
      const checkDigitInputs = document.getElementById("check-digit-inputs");
      const newNodeP = document.createElement("p");
      const nodePText = document.createTextNode(value);
      if (className === "error") {
        newNodeP.style.color = "red";
      } else if (className === "success") {
        newNodeP.style.color = "green";
      }
      newNodeP.appendChild(nodePText);
      checkDigitInputs.appendChild(newNodeP);
    },
    checkValidISBN13(data) {
      let sum = 0;
      for (let index = 0; index < data.length; index++) {
        index % 2 === 0 ? (sum += data[index] * 1) : (sum += data[index] * 3);
      }
      const joinedISBN = data.join("");
      sum % 10 === 0
          ? this.printIt(joinedISBN + " is a Valid ISBN number", "success")
          : this.printIt("Not a valid ISBN", "error");
    },
    clearMessages() {
      const checkDigitInputs = document.getElementById("check-digit-inputs");
      const childNodes = checkDigitInputs.childNodes;
      for (let i = 0; i < childNodes.length; i++) {
        if (i >= 1) {
          checkDigitInputs.removeChild(childNodes[i]);
        } else {
          i = 1
        }
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.check-digit-inputs {
  width: 30%;
  margin: auto;
}

#check-digit-input {
  font-size: 20px;
  margin: auto;
  border-radius: 0 0;
}

#check-digit-button {
  background: transparent;
  position: fixed;
  margin-left: -25px;
  font-size: 20px;
  padding: 4px;
  border: transparent;
}
.news{
  margin: auto;
  width: 300px;
}
</style>
