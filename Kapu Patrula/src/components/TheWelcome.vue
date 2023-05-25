<script setup>
import WelcomeItem from './WelcomeItem.vue'
import DocumentationIcon from './icons/IconDocumentation.vue'
import ToolingIcon from './icons/IconTooling.vue'
import EcosystemIcon from './icons/IconEcosystem.vue'
import CommunityIcon from './icons/IconCommunity.vue'
import SupportIcon from './icons/IconSupport.vue'
import Form from 'vform'

</script>
<template>
  <el-row>
    <el-col :span="24">
      <el-steps :active="activeStep" finish-status="success" align-center>
        <el-step title="Informācija par Jums" />
        <el-step title="Kas noticis?" />
        <el-step title="Kur noticis?" />
        <el-step title="Papildus informācija" />
      </el-steps>
    </el-col>
  </el-row>
  <br>

  <div class="sludinajuma-forma">
    <el-form :model="form" label-width="120px">
    <el-collapse v-model="step" @change="handleChange">
      <el-collapse-item title="Informācija par Jums" name="1">
        <div>
          <el-alert title="Šī informācija mums ir nepieciešama, lai ar Jums spētu sazināties Mūsu operātors" type="info" />
          <br>
        </div>
        <div>
          <el-form-item label="Jūsu vārds">
            <el-input v-model="form.firstname" />
          </el-form-item>
          <el-form-item label="Tālrunis">
            <el-input v-model="form.phone" />
          </el-form-item>
          <el-form-item label="E-pasts">
            <el-input v-model="form.email" />
          </el-form-item>
        </div>
      </el-collapse-item>
      <el-collapse-item title="Kas noticis?" name="2">
        <div class="demo-button-style">
          <el-form-item label="Kas noticies?">
            <el-radio-group v-model="eventState">
              <el-radio label="Atrasts">Atrasts</el-radio>
              <el-radio label="Pazudis">Pazudis</el-radio>
              <el-radio label="Meklē jaunas mājas">Meklē jaunas mājas</el-radio>
              <el-radio label="Klaiņo">Klaiņo</el-radio>
            </el-radio-group>
          </el-form-item>

          <el-divider />

          <el-form-item label="Dzīvnieks">
            <el-select v-model="animal" class="m-2" placeholder="Dzīvnieks">
              <el-option
                  v-for="item in animals"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
              />
            </el-select>
          </el-form-item>

        </div>
      </el-collapse-item>
      <el-collapse-item title="Kur noticis?" name="3">
        <div>
          Simplify the process: keep operating process simple and intuitive;
        </div>
        <div>
          Definite and clear: enunciate your intentions clearly so that the
          users can quickly understand and make decisions;
        </div>
        <div>
          Easy to identify: the interface should be straightforward, which helps
          the users to identify and frees them from memorizing and recalling.
        </div>
      </el-collapse-item>
      <el-collapse-item title="Papildus informācija" name="4">
        <div>
          Decision making: giving advices about operations is acceptable, but do
          not make decisions for the users;
        </div>
        <div>
          Controlled consequences: users should be granted the freedom to
          operate, including canceling, aborting or terminating current
          operation.
        </div>
      </el-collapse-item>

    </el-collapse>
<div>
  <br>
  <el-checkbox v-model="checked2" @change="toggleSubmitButtons($event)">Esmu iepazinies ar lietošanas noteikumiem</el-checkbox>
  <el-divider/>
  <el-form-item>
    <el-button type="success" :disabled="!canSubmit">Iesniegt sludinājumu</el-button>
    <el-button>Atcelt</el-button>
  </el-form-item>
</div>

    </el-form>
  </div>
</template>
<script>
import {ElMessage} from 'element-plus'
export default {
  name: 'Iesniegt sludinajumu',
  data() {
    return {
      form: new Form({
        firtname: '',
        email: '',
        phone: '',
      }),
      step: ['1'],
      canSubmit: false,
      activeStep: 4,
      eventState: '',
      animal: '',
      animals: [
        {
          value: 'Suns',
          label: 'Suns',
        },
        {
          value: 'Kaķis',
          label: 'Kaķis',
        },
        {
          value: 'Cits',
          label: 'Cits',
        },
      ]
    }
  },
  props: [
    'x',
  ],
  created() {
  },
  mounted() {
  },
  methods: {
    toggleSubmitButtons(event){
      if(event){
        this.canSubmit = true;
      } else {
        this.canSubmit = false;
      }
    }
  }
}
</script>