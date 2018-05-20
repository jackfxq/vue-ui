<template>
  <div class="docs-demo-wrapper">
      <div :style="{height: isExpand ? height+'px' : '0'}" class="demo-container">
        <div span="14">
          <div class="docs-demo docs-demo--expand">
            <div class="highlight-wrapper">
              <div class="code-content">
                <slot name="code"></slot>
              </div>
              <span class="run-code" @click="runCode">运行代码</span>
              <span class="copy-code" @click="copyCode">复制代码</span>
              <textarea type="text" class="copy-code-content" :value="code" style="opacity:0;position:absolute;">
              </textarea>
              <div class="highlight-content">
                <slot name="highlight"></slot>
              </div>
            </div>
          </div>
        </div>
      </div>
    <span class="docs-trans docs-demo__triangle" @click="toggle">{{isExpand ? '隐藏代码' : '显示代码'}}</span>
  </div>
</template>

<script>
  /* eslint-disable */
  import Vue from 'vue'
  export default {
    data() {
      return {
        isExpand: false,
        height:0,
        code:''
      };
    },
    watch:{
      isExpand() {
        console.log(this.$el.querySelector('.docs-demo--expand'))
      }
    },
    methods: {
      toggle() {
        this.isExpand = !this.isExpand;
      },
      runCode() {
        this.$nextTick(() => {

          window.open(`${location.href.split('#')[0]}#/code/${encodeURIComponent(this.code)}`)
          console.log(this.code)
        })

      },
      copyCode() {
        // window.clipboardData.setData("Text",this.code);
        var e = this.$el.querySelector('.copy-code-content');
        console.log(e.value)
        e.select();
        document.execCommand("copy",false,null);
        alert('复制成功')
      },
      setCode() {

      }
    },
    mounted() {
      this.height=this.$el.querySelector('.docs-demo--expand').offsetHeight;
      let codeStr = this.$el.querySelector('.code-content div').getAttribute('data-desc');
      let code = decodeURIComponent(codeStr).replace('<html><head></head><body>','').replace('</body></html>','')
      this.code=code;
      console.log(this.$el.querySelector('.docs-demo--expand').offsetHeight)
    }
  };
</script>

<style lang="scss" type="text/less">

  .demo-container {
    transition: height .5s ease;
    overflow: hidden;
  }
  .docs-demo {
    width: 100%;
    height: auto;
    box-sizing: border-box;
    font-size: 14px;
    background-color: #eee;
    border: 1px solid #e2ecf4;
    border-top: none;
    pre code {
      font-family: Consolas,Menlo,Courier,monospace;
      line-height: 22px;
      border: none;
    }
  }
  .docs-trans {
    width: 100%;
    text-align: center;
    display: inline-block;
    color: #ccc;
    font-size: 12px;
    padding: 10px 0;
    /* background-color: #FAFBFC; */
    cursor:pointer;
    border-bottom: 1px solid #eee;
    border-left: 1px solid #eee;
    border-right: 1px solid #eee;
    box-sizing: border-box;
    border-radius:0 0 4px 4px;
  }

  .docs-trans:hover{
    background-color: #FAFBFC;
    color:#3FAAF5;
  }

  .docs-demo__code,
  .highlight-wrapper,
  .docs-demo__meta {
    /* padding: 0 20px; */
    overflow-y: auto;
  }

  .docs-demo__code {
    border-bottom: 1px solid #eee;
  }
  .docs-demo.docs-demo--expand .docs-demo__meta {
    border-bottom: 1px dashed #e9e9e9;
  }

  .docs-demo.docs-demo--expand .docs-demo__triangle {
    transform: rotate(180deg);
  }

  .highlight-wrapper {
    display: none;

    p,
    pre {
      margin: 0;
    }

    .hljs {
      padding: 0;
    }
  }

  .docs-demo.docs-demo--expand .highlight-wrapper {
    display: block;
    position:relative;
    .run-code{
      position: absolute;
      top: 10px;
      right: 70px;
      color:#3FAAF5;
      cursor: pointer;
    }
    .copy-code{
      position: absolute;
      top: 10px;
      right: 10px;
      color:#3FAAF5;
      cursor: pointer;
    }
  }

  .docs-demo__code__mobi {
    height: 620px;
    margin: 20px 0;
  }

  .docs-demo__code__mobi__header {
    border-radius: 4px 4px 0 0;
    background: -webkit-linear-gradient(rgba(55,55,55,.98),#545456);
    background: linear-gradient(rgba(55,55,55,.98),#545456);
    text-align: center;
    padding: 8px;

    img {
      width: 100%;
    }

    .url-box {
      height: 28px;
      line-height: 28px;
      color: #fff;
      padding: 0 3px;
      background-color: #a2a2a2;
      margin: 10px auto 0;
      border-radius: 4px;
      white-space: nowrap;
      overflow-x: auto;
    }
  }

  .docs-demo__code__mobi__content {
    iframe {
      width: 100%;
      border: 0;
      height: 548px;
    }
  }

</style>
