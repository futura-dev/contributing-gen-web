<template>
  <div id="configurator" class="shadow rounded-lg pt-3 px-3 pb-1">
    <div class="d-flex">
      <h5>Project</h5>
    </div>
    <Input label="Organization Name" placeholder="Futura IT SRL" v-model="specs.project.organizationName" />
    <Input label="Project Name" placeholder="XYZ" v-model="specs.project.name" />
    <Input label="Project Slug" placeholder="xyz" v-model="specs.project.slug" />
    <Input label="Default Branch" placeholder="main" v-model="specs.project.defaultBranch" />
    <Input label="Repository URL" placeholder="https://github.com/user/project-slug" type="url"
      tooltip="The URL of your Git repository" v-model="specs.project.repoUrl" />
    <Input label="Documentation URL" placeholder="https://docs.xyz-project.io" type="url"
      tooltip="The URL of your documentation" v-model="specs.project.docsUrl" />

    <div class="d-flex">
      <h5>CONTRIBUTING.md</h5>
    </div>
    <InputSwitch label="Generate" tooltip="Should the CONTRIBUTING.md file be generated?"
      v-model="specs.contributing.generate" />
    <Input label="Security Email" placeholder="security@example.com" type="email"
      tooltip="Where would you like to be informed about sensitive bugs?" v-model="specs.contributing.emailSensitiveBugs"
      :disabled="!specs.contributing.generate" />

    <div class="d-flex">
      <h5>CODE_OF_CONDUCT.md</h5>
    </div>
    <InputSwitch label="Generate" tooltip="Should the CODE_OF_CONDUCT.md file be generated?"
      v-model="specs.codeOfConduct.generate" />

    <div class="d-flex">
      <h5>LICENSE.md</h5>
    </div>
    <InputSwitch label="Generate" tooltip="Should the LICENSE.md file be generated?" v-model="specs.license.generate" />

    <div class="d-flex">
      <h5>README.md</h5>
    </div>
    <InputSwitch label="Generate" tooltip="Should the README.md file be generated?" v-model="specs.readme.generate" />

    <Input label="Support Email" placeholder="support@example.com" type="email"
      tooltip="Where would you like to be informed for support purposes?" v-model="specs.readme.supportEmail" />
  </div>
</template>

<script>
import Input from "./Input";
import InputSwitch from "./InputSwitch";

export default {
  name: "Configurator",
  components: {
    Input,
    InputSwitch
  },
  methods: {
    generate() {
      this.$emit("generate", this.specs);
    },
  },
  watch: {
    specs: {
      handler: function () {
        // debounce the automatic generation while the user still types
        clearTimeout(this.timeoutId);
        this.timeoutId = setTimeout(this.generate, 500);
      },
      deep: true
    }
  },
  data() {
    return {
      specs: {
        project: {
          organizationName: "",
          name: "",
          slug: "",
          defaultBranch: "",
          repoUrl: "",
          docsUrl: ""
        },
        contributing: {
          generate: true,
          emailSensitiveBugs: ""
        },
        codeOfConduct: {
          generate: true,
        },
        license: {
          generate: true,
        },
        readme: {
          generate: true,
          supportEmail: ""
        }
      },
      projectSlugManuallyChanged: false,
      timeoutId: 0
    };
  }
};
</script>

<style scoped>
#configurator {
  background-color: var(--light);
}
</style>