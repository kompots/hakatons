<script setup>
import WelcomeItem from './WelcomeItem.vue'
import DocumentationIcon from './icons/IconDocumentation.vue'
import ToolingIcon from './icons/IconTooling.vue'
import EcosystemIcon from './icons/IconEcosystem.vue'
import CommunityIcon from './icons/IconCommunity.vue'
import SupportIcon from './icons/IconSupport.vue'
import MapView from "@/components/MapView.vue";
import Form from 'vform'
import { Picture as IconPicture, InfoFilled } from '@element-plus/icons-vue'
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
    <el-form :model="form" :rules="rules" label-position="top" id="sludinajums" label-width="220px!important">
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
          <el-form-item label="E-pasts" prop="email" type="email">
            <el-input type="email" v-model="form.email" />
          </el-form-item>
        </div>
      </el-collapse-item>
      <el-collapse-item title="Kas noticis?" :disabled="activeStep!==1" name="2">
        <div class="demo-button-style">
          <el-form-item label="Status" style="float: left; width: 75vw; margin-right: 16px;" prop="happened">
            <el-radio-group v-model="eventState">
              <el-radio label="Atrasts">Atrasts</el-radio>
              <el-radio label="Pazudis">Pazudis</el-radio>
              <el-radio label="Meklē jaunas mājas">Meklē jaunas mājas</el-radio>
              <el-radio label="Klaiņo">Klaiņo</el-radio>
            </el-radio-group>
          </el-form-item>
          <el-popover
              placement="left-start"
              :width="300"
              trigger="hover"
              content='"Meklē mājas" neattiecas uz atrastiem dzīvniekiem, bet uz tiem, kurus privātpersonas vēlas atdot jauniem saimniekiem.'>
            <template #reference>
              <el-icon>
                <InfoFilled/>
              </el-icon>
            </template>
          </el-popover>

          <el-form-item label="Dzīvnieks" prop="animal">
            <el-radio-group v-model="form.animal">
              <el-radio label="Suns">Suns</el-radio>
              <el-radio label="Kaķis">Kaķis</el-radio>
              <el-radio label="Cits">Cits</el-radio>
            </el-radio-group>
          </el-form-item>

        </div>
      </el-collapse-item>
      <el-collapse-item title="Kur noticis?"  :disabled="activeStep!==2" name="3">
        <div id="locationMap">
          <MapView />
        </div>
        <div id="manualadress">
          <el-form-item label="Pilsēta"  prop="city">
            <el-input v-model="form.city" />
          </el-form-item>
          <el-form-item label="Adrese" prop="address">
            <el-input v-model="form.address" />
          </el-form-item>
        </div>
        <br/>
        <el-form-item label="Notikuma datums" prop="date">
          <el-date-picker
              style="width: 83vw;!important;"
              v-model="form.date"
              type="date"
              placeholder="Izvēlieties datumu">
          </el-date-picker>
        </el-form-item>
      </el-collapse-item>
      <el-collapse-item title="Papildus informācija" :disabled="activeStep!==3" name="4">
        <div>
          <el-form-item label="Šķirne">
              <el-radio-group v-model="form.skirne">
                <el-radio
                    v-for="item in skirnes"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value"
                />
              </el-radio-group>
          </el-form-item>
          <el-form-item label="Dzimums">
            <el-radio-group v-model="form.dzimums">
              <el-radio
                  v-for="item in dzimums"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
              />
            </el-radio-group>
          </el-form-item>
          <el-form-item label="Vecums">

            <el-radio-group v-model="form.vecums">
              <el-radio
                  v-for="item in vecums"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
              />
            </el-radio-group>

          </el-form-item>
          <el-form-item label="Apzīmējuma veids">
            <el-radio-group v-model="form.apzimejums">
              <el-radio
                  v-for="item in apzimejumi"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
              />
            </el-radio-group>
          </el-form-item>
          <el-form-item label="Apzīmējums" prop="mark">
            <el-input v-model="form.apzimejumi" style="float: left; width: 75vw; margin-right: 16px;"/>
            <el-popover
                placement="left-start"
                :width="300"
                trigger="hover"
                content="Vai dzīvnieks ir kaut kādā veidā iezīmēts, piemēram, tetovēts? Ievadiet apzīmējuma numuru. Ja dzīvnieks ir čipēts, tad ievadiet mikroshēmas numuru.">
              <template #reference>
                <el-icon>
                  <InfoFilled/>
                </el-icon>
              </template>
            </el-popover>
          </el-form-item>
          <el-form-item label="Apraksts" prop="description">
            <el-input v-model="form.description" type="textarea" style="float: left; width: 75vw; margin-right: 16px;"/>
            <el-popover
                placement="left-start"
                :width="300"
                trigger="hover"
                content='Raksturojiet dzīvnieku īsi un precīzi, piemēram, "Ļoti gudrs - zin visas komandas. Bija iekļuvis satiksmes negadījumā, operēta priekšķepa".'>
              <template #reference>
                <el-icon>
                  <InfoFilled/>
                </el-icon>
              </template>
            </el-popover>
          </el-form-item>
          <el-divider content-position="left">Pievieno foto attēlus</el-divider>
          <el-upload style="position: relative; left: 50%; margin-left: -78px;"
                     action="https://f.kalni.app"
              list-type="picture-card"
              :on-preview="handlePictureCardPreview"
              >
            <el-icon><icon-picture /></el-icon>
          </el-upload>
          <el-dialog :visible.sync="dialogVisible">
            <img width="100%" :src="dialogImageUrl" alt="" id="blah">
          </el-dialog>
        </div>
      </el-collapse-item>

    </el-collapse>
<div>
  <br>
  <el-checkbox v-model="checked2" @change="toggleSubmitButtons($event)" :disabled="canFinish">Esmu iepazinies ar lietošanas noteikumiem</el-checkbox>
  <el-divider/>
  <el-form-item>
    <el-button type="success" :disabled="!canSubmit" @click="submitForm()">Iesniegt sludinājumu</el-button>
    <el-button @click="resetForm()">Atcelt</el-button>
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
        city: '',
        address: '',
        firstname: '',
        email: '',
        phone: '',
        skirne: '',
        animal: '',
        vecums: '',
        apzimejums: '',
        apzimejumi: '',
        date: '',
        dzimums: '',
        description: '',
        options: {
          enableHighAccuracy: true,
          timeout: 5000,
          maximumAge: 0,
        },
        center: {
          lat: 56.946285, lng: 24.105078
        },
        markers: [
          {
            position: {
              lat: 56.964310, lng: 24.134800
            }
          }
        ]
      }),
      dialogImageUrl: '',
      dialogVisible: false,
      canFinish: true,
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
          {require: true, trigger: 'change', validator: this.isSelectedHappened}
        ],
        animal: [
          {require: true, trigger: 'change', validator: this.isSelectedAnimal}
        ],
        date: [
          {required: true, message: 'Nepareizs datums noradīts', trigger: 'change', validator: this.isCorrectDate}
        ],
        mark: [
            {required: true, message: 'Nepareizi ievadīts apzimējums', trigger: 'change', validator: this.isCorrectMark}
        ],
        description: [
          {required: true, message: 'Nepareizi ievadīts apraksts', trigger: 'change', validator: this.isCorrectDescription}
        ],
      },
      step: ['1'],
      canSubmit: false,
      activeStep: 0,
      eventState: '',
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
      apzimejumi: [
        {
          value: 'Tetovējums',
          label: 'Tetovējums',
        },
        {
          value: 'Čips',
          label: 'Čips',
        },
        ],
      vecums: [
        {
          value: 'Nav zināms',
          label: 'Nav zinams',
        },
        {
          value: '1-6 mēneši',
          label: '1-6 mēneši',
        },
        {
          value: '7-12 mēneši',
          label: '7-12 mēneši',
        },
        {
          value: '1-3 gadi',
          label: '1-3 gadi',
        },
        {
          value: '4-8 gadi',
          label: '4-8 gadi',
        },
        {
          value: '8+ gadi',
          label: '>8 gadi',
        },
      ],
      skirnes: [
        {
          value: 'Bez šķirnes',
          label: 'Bez šķirnes',
        },
        {
          value: 'Šķirnes',
          label: 'Šķirnes',
        },
      ],
      dzimums: [
        {
          value: 'Mātīte',
          label: 'Mātīte',
        },
        {
          value: 'Tēviņš',
          label: 'Tēviņš',
        },
        {
          value: 'Sterilizēta mātīte',
          label: 'Sterilizēta mātīte',
        },
        {
          value: 'Kastrēts tēviņš',
          label: 'Kastrēts tēviņš',
        },
        {
          value: 'Nezināms',
          label: 'Nezināms',
        },
      ],
      isFirstName: false,
      isPhone: false,
      isEmail: false,
      isAnimal: false,
      isHappened: false,
      isDate: false
    }
  },

  props: [],
  created() {
  },
  mounted() {
    navigator.geolocation.getCurrentPosition(this.success, this.error, this.options);
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
    isSelectedHappened(event, value) {
      this.isHappend = value !== '';
      this.isWhatHappenedFullFilled();
      return this.isHappend;
    },
    isSelectedAnimal(event, value) {
      this.isAnimal = value !== '';
      this.isWhatHappenedFullFilled();
      return this.isAnimal;
    },
    isWhatHappenedFullFilled() {
      if (this.isHappend && this.isAnimal && this.activeStep === 1) {
        this.activeStep++;
        this.step.push("3");
      }
    },
    isCorrectDate(event, value) {
      this.isDate = new Date(value).getTime() <= new Date().getTime();
      this.isWhereHappenedFullFilled();
      this.canFinish = false
      return this.isDate;
    },
    isWhereHappenedFullFilled() {
      if (this.isDate && this.activeStep === 2) {
        this.activeStep++;
        this.step.push("4");
      }
    },
    isCorrectMark(event, value) {
      return /^[A-Z0-9]{3,32}$/i.test(value);
    },
    isCorrectDescription(event, value) {
      return /^[A-Z 0-9 a-z]*$/i.test(value);
    },
    handleRemove(file, fileList) {
      console.log(file, fileList);
    },
    handlePictureCardPreview(file) {
      this.dialogImageUrl = file.url;
      this.dialogVisible = true;
    },
    submitForm(){
      console.log(this.form)

    },
    resetForm(){
      document.getElementById("sludinajums").reset();
    },
    success(pos) {

      document.getElementById("manualadress").style.display = 'none'
    },
    error(err) {
      document.getElementById("locationMap").style.display = 'none'
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