<template lang="html">
  <div class="code-container">
    <div class="code-mirror-container">
      <div class="code-mirror"></div>
    </div>
    <div class="preview-demo">
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import CodeMirror from 'codemirror'
import 'codemirror/lib/codemirror.css'

import 'codemirror/mode/htmlmixed/htmlmixed'
import 'codemirror/theme/erlang-dark.css'
import 'codemirror/addon/edit/closebrackets.js'
import 'codemirror/addon/edit/matchbrackets.js'
import 'codemirror/addon/edit/closetag.js'
import 'codemirror/addon/edit/matchtags.js'

export default {
  data() {
    return {
      editor:null,
      vm:null
    }
  },
  methods:{
    renderDemo(demo) {
      // console.log(demo.split('\n').join(''))
      let str = demo.split('\n').join('');
      // let script = demo.match(/<script>(.*)<\/script>/);

      let htmlPattern = /<template>(.*)<\/template>/;
      let html=''
      let htmlArr = htmlPattern.exec(str);
      if(htmlArr&&htmlArr.length >1){
        html = htmlArr[1];
      }
      let scriptPattern = /<script>(.*)<\/script>/;
      let scriptArr = scriptPattern.exec(str);
      if(scriptArr&&scriptArr.length>1){
        exportStr = scriptArr[1];
      }
      let exportPattern = /export default(\s*)(.*)/;
      let exportStr = '';
      let optionsStr = '';
      let exportArr = exportPattern.exec(exportStr)
      if(exportArr&&exportArr.length >2){
        optionsStr = exportArr[2];
      }

      console.log(optionsStr)
      let options={}
      if(optionsStr){
        try {
          options=eval(`(${optionsStr})`)
        }catch(err){
          console.log(err)
        }
      }


      // console.log(eval(`window.a = ${optionsStr}`))

      console.log(options,optionsStr,html)
      let Demo = Vue.extend(Object.assign({
        template: `<div>${html}</div>`
      },options))
      this.vm = new Demo().$mount();
      console.log(this.vm)
      this.$el.querySelector('.preview-demo').innerHTML='';
      this.$el.querySelector('.preview-demo').appendChild(this.vm.$el);
    }
  },
  mounted() {
    // console.log(this.$route.params.code)
    let demo = this.$route.params.code;
    this.editor = CodeMirror(this.$el.querySelector('.code-mirror'),{
        value:demo,
        mode:'text/html',
        theme:'erlang-dark',
        styleActiveLine: true,
        lineNumbers:true,
        tabSize:2,
        lineWrapping: true,
        autofocus: true,
        matchBrackets: true,
		    autoCloseBrackets: true,

      })

      this.renderDemo(demo);

      this.editor.on('blur',() => {
        // this.vm.$destory();
        this.renderDemo(this.editor.getValue());

      })
  }
}
</script>

<style lang="scss">
  .code-container{
    margin-top: 80px;
    display: flex;
    height:calc(100vh - 80px);
    .code-mirror-container{
      width: 50%;
      .code-mirror{
        height: 100%
      }
    }
    .preview-demo{
      width: 50%;
      padding:20px
    }
  }
  .CodeMirror{
    height: 100%;
  }
</style>
