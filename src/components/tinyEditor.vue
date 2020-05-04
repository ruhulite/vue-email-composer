<template>
  <div class="user-editor">
    <editor
      id="userInput"
      api-key="no-api-key"
      :init="{
        selector: '#userInput',
        height: 350,
        setup: function(editor) {
          editor.ui.registry.addAutocompleter('autoCompleter', {
            ch: '{',
            minChars: 1,
            columns: 'auto',
            fetch: function(pattern) {
              const specialChars = [
                {text: '{first_name}', value: ''},
                {text: '{last_name}', value: ''},
                {text: '{country}', value: ''},
                {text: '{dollars}', value: ''},
                {text: '{percent sign}', value: ''},
                {text: '{caret}', value: ''},
                {text: '{ampersand}', value: ''},
                {text: '{asterisk}', value: ''},
              ];
              var matchedChars = specialChars.filter(function(char) {
                return char.text.indexOf(pattern) !== -1;
              });

              return new tinymce.util.Promise(function(resolve) {
                var results = matchedChars.map(function(char) {
                  return {
                    value: char.text,
                    text: char.text,
                    icon: char.text,
                  };
                });
                resolve(results);
              });
            },
            onAction: function(autocompleteApi, rng, value) {
              editor.selection.setRng(rng);
              editor.insertContent(value);
              autocompleteApi.hide();
            },
          });
        },
      }"
      v-model="message"
    />
    <button @click="handleClick">Submit</button>
  </div>
</template>

<script>
import Editor from '@tinymce/tinymce-vue';
//var Editor = require('@tinymce/tinymce-vue').default;

export function Activation() {
  let tinymce = require('@tinymce/tinymce-vue').default;
  return {
    selector: '#userInput',
    height: 350,
    setup: function(editor) {
      editor.ui.registry.addAutocompleter('autoCompleter', {
        ch: '{',
        minChars: 1,
        columns: 'auto',
        fetch: function(pattern) {
          const specialChars = [
            {text: '{first_name}', value: ''},
            {text: '{last_name}', value: ''},
            {text: '{country}', value: ''},
            {text: '{dollars}', value: ''},
            {text: '{percent sign}', value: ''},
            {text: '{caret}', value: ''},
            {text: '{ampersand}', value: ''},
            {text: '{asterisk}', value: ''},
          ];
          var matchedChars = specialChars.filter(function(char) {
            return char.text.indexOf(pattern) !== -1;
          });

          return new tinymce.util.Promise(function(resolve) {
            var results = matchedChars.map(function(char) {
              return {
                value: char.text,
                text: char.text,
                icon: char.text,
              };
            });
            resolve(results);
          });
        },
        onAction: function(autocompleteApi, rng, value) {
          editor.selection.setRng(rng);
          editor.insertContent(value);
          autocompleteApi.hide();
        },
      });
    },
  };
}

export default {
  name: 'tinyEditor',
  data: function() {
    return {
      message: '',
    };
  },
  components: {
    editor: Editor,
  },
  methods: {
    option: function() {
      return Activation();
    },
    handleClick: function() {
      //console.log('Click ', this.message);
      const data = [
        'first_name',
        'last_name',
        'address',
        'This is on this string...',
      ];
      let userInput = this.message;

      if (userInput && (userInput != null || userInput != '')) {
        let result = [],
          rxp = /{([^}]+)}/g,
          str = userInput,
          curMatch;
        while ((curMatch = rxp.exec(str))) {
          result.push(curMatch[1]);
        }
        const filterData = result.filter((text) => !data.includes(text));
        let newData = '';
        for (let i in filterData) {
          newData += filterData[i] + '*';
        }
        let newString = newData;
        let newStringArray = newString.split('*');
        let arrayFilter = newStringArray.filter((item) => item);

        let replaceArray = arrayFilter;
        let finalAns = userInput;
        for (let i = replaceArray.length - 1; i >= 0; i--) {
          finalAns = finalAns.replace(
            RegExp(
              '{' +
                replaceArray[i].replace(/[-/\\^$*+?.()|[\]{}]/g, '\\$&') +
                '}',
              'g'
            ),
            '<span style="color: red;">{' + replaceArray[i] + '}</span>'
          );
        }
        const finalAnsRevert = finalAns;
        this.message = finalAnsRevert;
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.user-editor {
  max-width: 45%;
  padding: 40px;
}
</style>
