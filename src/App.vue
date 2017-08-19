<template>
  <div id="app">
    <label for="original">your text (beware unicode)</label>
    <textarea v-model="original" @keyup="computeBrainfuck" name="original" rows="2" cols="80"></textarea>

    <label for="brainfuck">brainfuck</label>
    <textarea :value="brainfuck" name="brainfuck" rows="20" cols="80"></textarea>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      original: "",
      brainfuck: "",
    }
  },
  methods: {
    repeat(string, num) {
      let builder = [];
      for (let i = 0; i < num; i++) builder.push(string);
      return builder.join('');
    },
    computeBrainfuck(e) {
      // so apparently making an array and then joining it is faster than += on a string repeatedly
      let builder = [];

      // splitting even the surrogate pairs
      const text = this.original.split('');

      // bail
      if (text.length == 0) {
        this.brainfuck = "";
        return
      }

      // convert to charcodes
      const textArray = text.map(function(char){
        return char.charCodeAt(0);
      });

      // add an appropriate multiple of 10 to each cell
      builder.push(this.repeat('+', 10));
      builder.push("\n[");

      for (let i = 0; i < textArray.length; i++) {
        builder.push("\n    > ");
        builder.push(this.repeat("+", Math.floor(textArray[i] / 10)));
      }
      builder.push("\n    ");
      // go back to first cell
      builder.push(this.repeat("<", textArray.length));
      builder.push(" -\n]\n> ");

      // add charcode - min for each character
      for (let i = 0; i < textArray.length; i++) {
        const charcode = textArray[i];
        builder.push(this.repeat('+', charcode % 10));
        builder.push(' . ');
        // alignment
        builder.push(this.repeat(' ', 9 - charcode % 10));
        builder.push(text[i]);

        if (i != textArray.length - 1) {
          builder.push("\n> ");
        }
      }

      this.brainfuck = builder.join('');
      return;
    }
  }
}
</script>

<style lang="css">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  display: flex;
  align-items: center;
  flex-direction: column;
  max-width: 50em;
}

label {
  margin-top: 2em;
}

textarea {
  display: block;
}
</style>
