<template>
  <div id="app">
    <Header />
    <div class="container-fluid">
      <div class="row">
        <div class="col-12 col-lg-6 col-xl-5 pr-auto pr-lg-0 mb-3">
          <Configurator @generate="generate($event)" />
        </div>
        <div class="col-12 col-lg-6 col-xl-7">
          <MarkdownOutput :tabs="markdownOutput" />
        </div>
      </div>
    </div>
    <Footer />
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Configurator from "./components/Configurator.vue";
import MarkdownOutput from "./components/MarkdownOutput.vue";

import { ContributingGen } from "@futura-dev/contributing-gen"
import contributingTemplate from "raw-loader!@futura-dev/contributing-gen/dist/templates/contributing.dot";
import codeOfConductTemplate from "raw-loader!@futura-dev/contributing-gen/dist/templates/codeOfConduct.dot";
import licenseTemplate from "raw-loader!@futura-dev/contributing-gen/dist/templates/license.dot";
import readmeTemplate from "raw-loader!@futura-dev/contributing-gen/dist/templates/readme.dot";
import welcomeMessage from "raw-loader!./assets/welcomeMessage.md";

export default {
  name: "App",
  components: {
    Header,
    Configurator,
    MarkdownOutput
  },
  data() {
    return {
      contributingGen: new ContributingGen(contributingTemplate, codeOfConductTemplate, licenseTemplate, readmeTemplate),
      markdownOutput: [
        {
          title: "Welcome",
          markdown: welcomeMessage,
          noDownloadButton: true
        }
      ]
    };
  },
  methods: {
    generate(specs) {
      this.markdownOutput = [];

      if (
        !specs.project.name ||
        !(specs.contributing.generate || specs.codeOfConduct.generate)
      ) {
        // If the specs are cleaned by the user, show the welcome message again
        this.markdownOutput.push({
          title: "Welcome",
          markdown: welcomeMessage,
          noDownloadButton: true
        });
      } else {
        // Otherwise if the user made inputs, generate the files
        if (specs.contributing.generate) {
          let markdown = this.contributingGen.generateContributing(specs);
          this.markdownOutput.push({
            title: "CONTRIBUTING.md",
            markdown: markdown
          });
        }
        if (specs.codeOfConduct.generate) {
          let markdown = this.contributingGen.generateCodeOfConduct(specs);
          this.markdownOutput.push({
            title: "CODE_OF_CONDUCT.md",
            markdown: markdown
          });
        }
        if (specs.license.generate) {
          let markdown = this.contributingGen.generateLicense(specs);
          this.markdownOutput.push({
            title: "LICENSE.md",
            markdown: markdown
          });
        }
        if (specs.readme.generate) {
          let markdown = this.contributingGen.generateReadme(specs);
          this.markdownOutput.push({
            title: "README.md",
            markdown: markdown
          });
        }
      }
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
</style>
