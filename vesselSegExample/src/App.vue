<template>
  <div id="app">
    <div>
      <select v-model="selectedSubjectKey">
        <option v-for="(subject, key) in subjectData" :key="key" :value="key">
          {{ key }}
        </option>
      </select>
      Subject: {{ selectedSubjectKey }}
      Age: {{ subjectData[selectedSubjectKey]?.age }}
      <select v-model="selectedScanType" :disabled="!selectedSubjectKey">
        <option v-for="(url, scanType) in subjectData[selectedSubjectKey]?.scans" :key="scanType" :value="scanType">
          {{ scanType }}
        </option>
      </select>
    </div>
    <NiiVueViewer
      v-if="selectedSubjectKey && selectedScanType"
      :key="`${selectedSubjectKey}-${selectedScanType}`"
      :imageUrl="subjectData[selectedSubjectKey].scans[selectedScanType]"
    />
    <p v-if="selectedSubjectKey && selectedScanType">
      Selected Subject: {{ selectedSubjectKey }},
      Age: {{ subjectData[selectedSubjectKey].age }},
      Scan Type: {{ selectedScanType }}
    </p>
  </div>
</template>

<script>
import NiiVueViewer from './components/NiiVueViewer.vue'

export default {
  name: 'App',
  components: {
    NiiVueViewer
  },
  data() {
    return {
      subjectData: {
        'IXI060': {
          age: 32,
          scanner: "Guys Philips 1.5T ",
          scannerLink: "https://brain-development.org/scanner-philips-medical-systems-gyroscan-intera-1-5t/",
          diceScore: "0.7589386261261262",
          scans: {
            'T1': 'https://raw.githack.com/asagilmore/VesselSegExample/main/IXI060_T1.nii.gz',
            'T2': 'https://raw.githack.com/asagilmore/VesselSegExample/main/IXI060_T2.nii.gz',
            'Prediced_VesselMap': 'https://raw.githack.com/asagilmore/VesselSegExample/main/IXI060_pred.nii.gz',
            'GroundTruth_VesselMap': 'https://raw.githack.com/asagilmore/VesselSegExample/main/IXI060.nii.gz'
          }
        },
        'IXI138': {
          age: 28,
          scanner: "Guys Philips 1.5T ",
          scannerLink: "https://brain-development.org/scanner-philips-medical-systems-gyroscan-intera-1-5t/",
          diceScore: "0.08452790441236978",
          scans: {
            'T1': 'https://raw.githack.com/asagilmore/VesselSegExample/main/IXI138_T1.nii.gz',
            'T2': 'https://raw.githack.com/asagilmore/VesselSegExample/main/IXI138_T2.nii.gz',
            'Predicted_VesselMap': 'https://raw.githack.com/asagilmore/VesselSegExample/main/IXI138_pred.nii.gz',
            'GroundTruth_VesselMap': 'https://raw.githack.com/asagilmore/VesselSegExample/main/IXI138.nii.gz'
          }
        },
      },
      selectedSubjectKey: 'IXI060',
      selectedScanType: 'T1'
    }
  },
  watch: {
    selectedSubjectKey(newValue) {
      if (newValue && this.subjectData[newValue].scans) {
        this.selectedScanType = Object.keys(this.subjectData[newValue].scans)[0];
      } else {
        this.selectedScanType = '';
      }
    }
  }
}
</script>

<style scoped>
select {
  margin-right: 10px;
  margin-bottom: 20px;
}
</style>
