<template>
  <div class="first-step-scenario-b">
    <form
      :class="{'is-invalid': isInvalid}" 
      @submit.stop.prevent="onSubmit"
    >
      <div class="first-step-scenario-b--content row">
      
        <div class="col-sm-8">
          <div class="text-brand-gray">
            <h2 class="mt-0">Καταχώρηση οχήματος</h2>
            <h4 class="font-weight-normal">Χρειαζόμαστε κάποιες πληροφορίες για τον οδηγό και το όχημα και θα βρούμε για σένα τις καλύτερες  προσφορές</h4>

            <br/>
            
            <h2>Στοιχεία οχήματος</h2>
          </div>      

          <!-- Vehicle Details -->
          <vehicle-detail-fields
            :userVehicle="userVehicle"          
          />

          <br/>

          <!-- Vehicle Used -->
          <div class="mt-3" v-if="userVehicle.make.length > 0">
            <vehicle-used-fields
              :userVehicle="userVehicle"          
            />
          </div>
          
          <!-- Driver Details -->
          <driver-detail-fields
            :userVehicle="userVehicle"          
          />
          
          <div class="first-step-scenario-b--action">
            <button type="button" class="btn btn-outline-secondary sm-padding" @click="goBack">       
              <svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path fill-rule="evenodd" clip-rule="evenodd" d="M9.27962 3.26455L4 7.99943L9.27962 12.7353C9.69721 13.1113 10.3474 13.0833 10.7311 12.6732C11.1128 12.2641 11.0853 11.6272 10.6667 11.2523L7.0426 7.99943L10.6667 4.7486C11.0853 4.37267 11.1128 3.73784 10.7311 3.32773C10.3474 2.91659 9.69721 2.88862 9.27962 3.26455ZM16 7.99951C16 12.4184 12.4177 16 8.00098 16C3.58234 16 0 12.4184 0 7.99951C0 3.58158 3.58234 0 8.00098 0C12.4177 0 16 3.58158 16 7.99951Z" fill="#F26E22"/>
              </svg>
            </button>
            <button type="submit" class="btn btn-secondary sm-padding">
              <span>
                <small>Συνεχεια</small>
                <br/>
                Επιλογη πακετου
              </span>
              <svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path fill-rule="evenodd" clip-rule="evenodd" d="M7.08963 3.26455L12.6593 7.99943L7.08963 12.7353C6.6491 13.1113 5.9632 13.0833 5.55835 12.6732C5.15574 12.2641 5.18474 11.6272 5.62639 11.2523L9.44957 7.99943L5.62639 4.7486C5.18474 4.37267 5.15574 3.73784 5.55835 3.32773C5.9632 2.91659 6.6491 2.88862 7.08963 3.26455ZM0 7.99951C0 12.4184 3.58234 16 7.99902 16C12.4177 16 16 12.4184 16 7.99951C16 3.58158 12.4177 0 7.99902 0C3.58234 0 0 3.58158 0 7.99951Z" fill="white"/>
              </svg>
            </button>
          </div>
        </div>

        <div class="col-sm-4">
          
          <!-- Title Section --->
          <title-section />

          <!-- Vehicle Review -->
          <vehicle-review-section
            :userVehicle="userVehicle" 
            class="hidden-xs"         
          />

          <!-- Info Sponsors Section -->
          <info-sponsors-section 
            class="hidden-xs"
          />

        </div>
      </div>
    </form>
  </div>
</template>

<script>
import mixinCommon from '@/helpers/mixinCommon'
import driverDetailFields from '@/components/steps/common/driverDetailFields.vue';
import vehicleDetailFields from '@/components/steps/common/vehicleDetailFields.vue';
import vehicleUsedFields from '@/components/steps/common/vehicleUsedFields.vue';
import vehicleReviewSection from '@/components/steps/common/vehicleReviewSection.vue';
import titleSection from '@/components/steps/common/titleSection.vue';
import infoSponsorsSection from '@/components/steps/common/infoSponsorsSection.vue';

export default {
  name: 'firstStepScenarioB',
  components: { 
    driverDetailFields, 
    vehicleDetailFields, 
    vehicleUsedFields, 
    vehicleReviewSection,
    titleSection,
    infoSponsorsSection
  },
  mixins: [mixinCommon],
  props: {
    steps: {
      type: Object,
      required: true
    },
    userVehicle: {
      type: Object,
      required: true
    },
  },
  data() {
    return {
      isInvalid: false,

      vehicleUsedOptions: [
        { text: 'NAI', value: 'no' },
        { text: 'OXI', value: 'yes' },
      ],
      hasAddtionalDriver: false,
    }
  },
  provide() {
    return {
      $validator: this.$validator,
    }
  },
  methods: {
    goBack() {
      this.isInvalid = false;
      this.$set(this.steps, 'modalStep1Show', true);
      Object.keys(this.userVehicle).forEach(e => { if(e !== 'licensePlate') this.$set(this.userVehicle, e, ''); })
      
      if(!this.steps.vehicleFound)
        this.$router.push({ name: 'pageHome' })
    },
    onSubmit() {
      
      this.$validator.validateAll().then(result => {
        if (!result) {
          this.isInvalid = true;
          return;
        }

        alert('success!')
      });
    }
  },
  mounted() {

  },
};
</script>

<style lang="scss" scoped>
.first-step-scenario-b {
  padding: 2rem 0;
}

.first-step-scenario-b--action {
  display: flex;
  margin-top: 1.875rem;

  > .btn {
    display: flex;
    align-items: center;
    justify-content: space-between;
    text-align: left;
    line-height: 1;
  }
  > .btn:first-child {
    justify-content: center;
    min-width: 55px;
    margin-right: 0.5rem;
    box-shadow: 0px 4px 16px rgba(0, 0, 0, 0.15);
  }
  > .btn:last-child {
    width: 100%;
    max-width: 275px;
  }
  > .btn:last-child span{
    font-size: 1.125rem;
  }
  > .btn:last-child small{
    font-weight: normal;
    font-size: 0.75rem;
  }
  > .btn:last-child svg {
    margin-left: 0.5rem;
  }
}

@media only screen and (max-width: 767px) {
  
  .first-step-scenario-b--content {
    display: flex;
    flex-direction: column-reverse;

    > .col-sm-4 {
      margin-bottom: 2rem;
    }
  }
}
</style>