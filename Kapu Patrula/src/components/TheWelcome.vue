<script setup>
import WelcomeItem from './WelcomeItem.vue'
import DocumentationIcon from './icons/IconDocumentation.vue'
import ToolingIcon from './icons/IconTooling.vue'
import EcosystemIcon from './icons/IconEcosystem.vue'
import CommunityIcon from './icons/IconCommunity.vue'
import SupportIcon from './icons/IconSupport.vue'
import Form from 'vform'
import { Picture as IconPicture } from '@element-plus/icons-vue'
import 'element-plus/theme-chalk/display.css'
</script>
<template>
  <el-row>
    <el-col :span="24" class="hidden-sm-and-down">
      <el-steps :active="activeStep" finish-status="success" align-center >
        <el-step title="Informācija par Jums" />
        <el-step title="Kas noticis?" />
        <el-step title="Kur noticis?" />
        <el-step title="Papildus informācija" />
      </el-steps>
    </el-col>
  </el-row>
  <br>

  <div class="sludinajuma-forma">
    <el-form :model="form" :rules="rules" label-position="top" label-width="220px!important">
    <el-collapse v-model="step" @change="handleChange">
      <el-collapse-item title="Informācija par Jums" name="1">
        <div>
          <el-alert title="Šī informācija mums ir nepieciešama, lai ar Jums spētu sazināties Mūsu operātors" type="info" />
          <br>
        </div>
        <div>
          <el-form-item label="Jūsu vārds" prop="firstname">
            <el-input v-model="form.firstname" />
          </el-form-item>
          <el-form-item label="Tālrunis" prop="phone">
            <el-input v-model="form.phone" />
          </el-form-item>
          <el-form-item label="E-pasts" prop="email">
            <el-input v-model="form.email" />
          </el-form-item>
        </div>
      </el-collapse-item>
      <el-collapse-item title="Kas noticis?" :disabled="activeStep!==1" name="2">
        <div class="demo-button-style">
          <el-form-item label="Kas noticies?" prop="happened">
            <el-radio-group v-model="eventState">
              <el-radio label="Atrasts">Atrasts</el-radio>
              <el-radio label="Pazudis">Pazudis</el-radio>
              <el-radio label="Meklē jaunas mājas">Meklē jaunas mājas</el-radio>
              <el-radio label="Klaiņo">Klaiņo</el-radio>
            </el-radio-group>
          </el-form-item>

          <el-divider />

          <el-form-item label="Dzīvnieks" >
            <el-select v-model="animal" class="m-2" placeholder="Dzīvnieks" style="width: 83vw;">
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
      <el-collapse-item title="Kur noticis?"  :disabled="activeStep!==2" name="3">
        <el-form-item label="Notikuma datums">
          <el-date-picker
              v-model="value1"
              type="date"
              placeholder="Pick a day">
          </el-date-picker>
        </el-form-item>
      </el-collapse-item>
      <el-collapse-item title="Papildus informācija" :disabled="activeStep!==3" name="4">
        <div>
          <el-form-item label="Šķirne">
            <el-input v-model="form.email" />
          </el-form-item>
          <el-form-item label="Dzimums">
            <el-input v-model="form.email" />
          </el-form-item>
          <el-form-item label="Vecums">
            <el-input v-model="form.email" />
          </el-form-item>
          <el-form-item label="Apzīmējuma veids">
            <el-input v-model="form.email" />
          </el-form-item>
          <el-form-item label="Apzīmējums">
            <el-input v-model="form.email" />
          </el-form-item>
          <el-form-item label="Apraksts">
            <el-input v-model="form.email" type="textarea" />
          </el-form-item>
          <el-divider content-position="left">Pievieno foto attēlus</el-divider>
          <el-upload
              action="http://127.0.0.1:5173/pic"
              list-type="picture-card"
              :on-preview="handlePictureCardPreview"
              :on-remove="handleRemove">
            <el-icon><icon-picture /></el-icon>
          </el-upload>
          <el-dialog :visible.sync="dialogVisible">
            <img width="100%" :src="dialogImageUrl" alt="">
          </el-dialog>
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
import Form from "vform";
export default {
  name: 'Iesniegt sludinajumu',
  data() {
    return {
      form: new Form({
        firstname: '',
        email: '',
        phone: '',
      }),
      pickerOptions: {
        disabledDate(time) {
          return time.getTime() > Date.now();
        },
        shortcuts: [{
          text: 'Today',
          onClick(picker) {
            picker.$emit('pick', new Date());
          }
        }, {
          text: 'Yesterday',
          onClick(picker) {
            const date = new Date();
            date.setTime(date.getTime() - 3600 * 1000 * 24);
            picker.$emit('pick', date);
          }
        }, {
          text: 'A week ago',
          onClick(picker) {
            const date = new Date();
            date.setTime(date.getTime() - 3600 * 1000 * 24 * 7);
            picker.$emit('pick', date);
          }
        }]
      },
      value1: '',
      value2: '',
      rules: {
        firstname: [
          {required: true, message: 'Nav derīgs vārds', trigger: 'change', validator: this.validateFirstname}
        ],
        phone: [
          {required: true, message: 'Nav derīgs mobilā telefona numurs', trigger: 'change', validator: this.validatePhone}
        ],
        email: [
          {required: true, message: 'Nav derīga e-pasta adrese', trigger: 'change', validator: this.validateEmail}
        ],
        happened: [
          {require: true, trigger: 'change', validator: this.isSelectedChoice}
        ],
      },
      step: ['1'],
      canSubmit: false,
      activeStep: 0,
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
      ],
      isFirstName: false,
      isPhone: false,
      isEmail: false
    }
  },

  props: [],
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
    },
    validateFirstname(event, value) {
      this.isFirstName = /^[a-z ,.'-]+$/i.test(value);
      this.validateContactInfo();
      return this.isFirstName;
    },
    validatePhone(event, value) {
      this.isPhone = /^[+]?[0-9]{0,3}[-\s.]?[0-9]{8}$/i.test(value);
      this.validateContactInfo();
      return this.isPhone;
    },
    validateEmail(event, value) {
      this.isEmail = /^[^@ \t\r\n]+@[^@ \t\r\n]+\.[^@ \t\r\n]+/i.test(value);
      this.validateContactInfo();
      return this.isEmail;
    },
    validateContactInfo() {
      if (this.isFirstName && this.isPhone && this.isEmail && this.activeStep === 0) {
        this.activeStep++;
        this.step.push("2");
      }
    },
    isSelectedChoice(event, value) {
      return this.eventState != '';
    },
    handleRemove(file, fileList) {
      console.log(file, fileList);
    },
    handlePictureCardPreview(file) {
      this.dialogImageUrl = file.url;
      this.dialogVisible = true;
    }
  }
}
</script>

<style >
.demo-image__error .image-slot {
  font-size: 30px;
}
.demo-image__error .image-slot .el-icon {
  font-size: 30px;
}
.demo-image__error .el-image {
  width: 100%;
  height: 100px;
}
label{
  width: 200px!important;
}
</style>