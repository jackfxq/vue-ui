<template lang="html">
  <div class="code-container">
    <vuep :template="code"></vuep>
  </div>
</template>

<script>

export default {
  data() {
    return {
      code:''
    }
  },
  methods:{
    renderDemo(demo) {
      let Demo = Vue.extend({
        template: `<div>${demo}</div>`
      })
      this.vm = new Demo().$mount();
      console.log(this.vm)
      this.$el.querySelector('.preview-demo').innerHTML='';
      this.$el.querySelector('.preview-demo').appendChild(this.vm.$el);
    }
  },
  mounted() {
    console.log(this.$route.params.code)
    let demo = this.$route.params.code;
    // this.code=demo;
    this.code = `
    <template>
        <div>Hello, {{ name }}!</div>
      </template>

    `
  }
}
</script>

<style lang="scss">
  .code-container{
    margin-top: 80px;
    // display: flex;
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
