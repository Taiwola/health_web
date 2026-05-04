<template>
  <div class="container mt-5 mb-5">
    <div class="row justify-content-center">
      <div class="col-12 col-lg-10">
        <div class="card shadow-sm">
          <div class="card-header bg-success text-white py-3">
            <h2 class="mb-0">
              <i class="fas fa-exclamation-triangle mr-2"></i>
              Report Adverse Drug Reaction
            </h2>
          </div>
          
          <div class="card-body p-4">

            <!-- Error Message -->
            <div v-if="errorMessage" class="alert alert-danger mb-4" role="alert">
              <i class="fas fa-exclamation-circle mr-2"></i>
              <strong>Submission failed:</strong>
              <ul class="mb-0 mt-1">
                <li v-for="(msg, field) in errorMessage" :key="field">
                  <strong>{{ field }}:</strong> {{ Array.isArray(msg) ? msg.join(', ') : msg }}
                </li>
              </ul>
            </div>

            <form @submit.prevent="submitReport">
              
              <!-- Section 1: Patient Information -->
              <div class="section-box mb-4">
                <h4 class="section-title">
                  <i class="fas fa-user mr-2" style="color: #258576;"></i>
                  Patient Information
                </h4>
                <div class="row">
                  <div class="col-md-6 mb-3">
                    <label class="form-label">Patient Name / Initials <span class="text-muted">(optional)</span></label>
                    <input type="text" class="form-control" v-model="form.patient.name" placeholder="e.g., J.D. or John Doe">
                  </div>
                  <div class="col-md-3 mb-3">
                    <label class="form-label">Age</label>
                    <input type="number" class="form-control" v-model="form.patient.age" placeholder="Years">
                  </div>
                  <div class="col-md-3 mb-3">
                    <label class="form-label">Gender</label>
                    <select class="form-select" v-model="form.patient.gender">
                      <option value="">Select</option>
                      <option value="male">Male</option>
                      <option value="female">Female</option>
                      <option value="unknown">Unknown</option>
                    </select>
                  </div>
                </div>
                <div class="row">
                  <div class="col-md-4 mb-3">
                    <label class="form-label">Weight (kg) <span class="text-muted">(optional)</span></label>
                    <input type="number" class="form-control" v-model="form.patient.weight" placeholder="kg">
                  </div>
                </div>
              </div>

              <!-- Section 2: Suspected Product -->
              <div class="section-box mb-4">
                <h4 class="section-title">
                  <i class="fas fa-pills mr-2" style="color: #258576;"></i>
                  Suspected Product
                </h4>
                <div class="row">
                  <div class="col-md-6 mb-3">
                    <label class="form-label">Drug Name <span class="text-danger">*</span></label>
                    <input type="text" class="form-control" v-model="form.product.drugName" required placeholder="Brand and generic name">
                  </div>
                  <div class="col-md-3 mb-3">
                    <label class="form-label">Batch/Lot Number</label>
                    <input type="text" class="form-control" v-model="form.product.batchNumber" placeholder="If available">
                  </div>
                  <div class="col-md-3 mb-3">
                    <label class="form-label">Expiry Date</label>
                    <input type="date" class="form-control" v-model="form.product.expiryDate">
                  </div>
                </div>
                <div class="row">
                  <div class="col-md-6 mb-3">
                    <label class="form-label">Manufacturer</label>
                    <input type="text" class="form-control" v-model="form.product.manufacturer" placeholder="Company name">
                  </div>
                  <div class="col-md-3 mb-3">
                    <label class="form-label">Dosage Form</label>
                    <select class="form-select" v-model="form.product.dosageForm">
                      <option value="">Select</option>
                      <option value="tablet">Tablet</option>
                      <option value="capsule">Capsule</option>
                      <option value="syrup">Syrup</option>
                      <option value="injection">Injection</option>
                      <option value="cream">Cream/Ointment</option>
                      <option value="drops">Drops</option>
                      <option value="other">Other</option>
                    </select>
                  </div>
                  <div class="col-md-3 mb-3">
                    <label class="form-label">Dose & Frequency</label>
                    <input type="text" class="form-control" v-model="form.product.doseFrequency" placeholder="e.g., 500mg twice daily">
                  </div>
                </div>
                <div class="row">
                  <div class="col-md-6 mb-3">
                    <label class="form-label">Route of Administration</label>
                    <select class="form-select" v-model="form.product.route">
                      <option value="">Select</option>
                      <option value="oral">Oral</option>
                      <option value="iv">Intravenous (IV)</option>
                      <option value="im">Intramuscular (IM)</option>
                      <option value="sc">Subcutaneous (SC)</option>
                      <option value="topical">Topical</option>
                      <option value="inhalation">Inhalation</option>
                      <option value="other">Other</option>
                    </select>
                  </div>
                  <div class="col-md-3 mb-3">
                    <label class="form-label">Start Date</label>
                    <input type="date" class="form-control" v-model="form.product.startDate">
                  </div>
                  <div class="col-md-3 mb-3">
                    <label class="form-label">Stop Date</label>
                    <input type="date" class="form-control" v-model="form.product.stopDate">
                  </div>
                </div>
              </div>

              <!-- Section 3: Adverse Reaction Details -->
              <div class="section-box mb-4">
                <h4 class="section-title">
                  <i class="fas fa-notes-medical mr-2" style="color: #258576;"></i>
                  Adverse Reaction Details
                </h4>
                <div class="row">
                  <div class="col-12 mb-3">
                    <label class="form-label">Description of Reaction <span class="text-danger">*</span></label>
                    <textarea class="form-control" v-model="form.reaction.description" rows="4" required 
                              placeholder="Please describe what happened, including symptoms, affected body parts, etc."></textarea>
                  </div>
                </div>
                <div class="row">
                  <div class="col-md-3 mb-3">
                    <label class="form-label">Date Reaction Started</label>
                    <input type="date" class="form-control" v-model="form.reaction.startDate">
                  </div>
                  <div class="col-md-3 mb-3">
                    <label class="form-label">Date Reaction Ended</label>
                    <input type="date" class="form-control" v-model="form.reaction.endDate">
                  </div>
                  <div class="col-md-3 mb-3">
                    <label class="form-label">Severity</label>
                    <select class="form-select" v-model="form.reaction.severity">
                      <option value="">Select</option>
                      <option value="mild">Mild</option>
                      <option value="moderate">Moderate</option>
                      <option value="severe">Severe</option>
                    </select>
                  </div>
                  <div class="col-md-3 mb-3">
                    <label class="form-label">Outcome</label>
                    <select class="form-select" v-model="form.reaction.outcome">
                      <option value="">Select</option>
                      <option value="recovered">Recovered</option>
                      <option value="recovering">Recovering</option>
                      <option value="not-recovered">Not Recovered</option>
                      <option value="unknown">Unknown</option>
                    </select>
                  </div>
                </div>
                
                <!-- Seriousness Criteria -->
                <div class="mb-3">
                  <label class="form-label fw-bold">Seriousness Criteria (check all that apply):</label>
                  <div class="row">
                    <div class="col-md-4">
                      <div class="form-check">
                        <input class="form-check-input" type="checkbox" v-model="form.reaction.seriousness.death" id="death">
                        <label class="form-check-label" for="death">Death</label>
                      </div>
                    </div>
                    <div class="col-md-4">
                      <div class="form-check">
                        <input class="form-check-input" type="checkbox" v-model="form.reaction.seriousness.lifeThreatening" id="lifeThreat">
                        <label class="form-check-label" for="lifeThreat">Life-threatening</label>
                      </div>
                    </div>
                    <div class="col-md-4">
                      <div class="form-check">
                        <input class="form-check-input" type="checkbox" v-model="form.reaction.seriousness.hospitalization" id="hospital">
                        <label class="form-check-label" for="hospital">Hospitalization</label>
                      </div>
                    </div>
                    <div class="col-md-4">
                      <div class="form-check">
                        <input class="form-check-input" type="checkbox" v-model="form.reaction.seriousness.disability" id="disability">
                        <label class="form-check-label" for="disability">Disability</label>
                      </div>
                    </div>
                    <div class="col-md-4">
                      <div class="form-check">
                        <input class="form-check-input" type="checkbox" v-model="form.reaction.seriousness.congenital" id="congenital">
                        <label class="form-check-label" for="congenital">Congenital Anomaly</label>
                      </div>
                    </div>
                    <div class="col-md-4">
                      <div class="form-check">
                        <input class="form-check-input" type="checkbox" v-model="form.reaction.seriousness.other" id="other">
                        <label class="form-check-label" for="other">Other Medically Important</label>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <!-- Section 4: Reporter Information -->
              <div class="section-box mb-4">
                <h4 class="section-title">
                  <i class="fas fa-user-md mr-2" style="color: #258576;"></i>
                  Reporter Information
                </h4>
                <div class="row">
                  <div class="col-md-6 mb-3">
                    <label class="form-label">Your Name <span class="text-danger">*</span></label>
                    <input type="text" class="form-control" v-model="form.reporter.name" required>
                  </div>
                  <div class="col-md-6 mb-3">
                    <label class="form-label">Reporter Type <span class="text-danger">*</span></label>
                    <select class="form-select" v-model="form.reporter.type" required>
                      <option value="">Select</option>
                      <option value="healthcare">Healthcare Professional</option>
                      <option value="patient">Patient</option>
                      <option value="consumer">Consumer/Caregiver</option>
                    </select>
                  </div>
                </div>
                <div class="row">
                  <div class="col-md-6 mb-3">
                    <label class="form-label">Phone Number <span class="text-danger">*</span></label>
                    <input type="tel" class="form-control" v-model="form.reporter.phone" required>
                  </div>
                  <div class="col-md-6 mb-3">
                    <label class="form-label">Email Address <span class="text-danger">*</span></label>
                    <input type="email" class="form-control" v-model="form.reporter.email" required>
                  </div>
                </div>
              </div>

              <!-- Section 5: Additional Information -->
              <div class="section-box mb-4">
                <h4 class="section-title">
                  <i class="fas fa-file-medical mr-2" style="color: #258576;"></i>
                  Additional Information
                </h4>
                <div class="row">
                  <div class="col-12 mb-3">
                    <label class="form-label">Concomitant Medications</label>
                    <textarea class="form-control" v-model="form.additional.concomitantMeds" rows="2" 
                              placeholder="List any other medications taken at the same time"></textarea>
                  </div>
                </div>
                <div class="row">
                  <div class="col-12 mb-3">
                    <label class="form-label">Relevant Medical History</label>
                    <textarea class="form-control" v-model="form.additional.medicalHistory" rows="2" 
                              placeholder="Include any relevant medical conditions, allergies, etc."></textarea>
                  </div>
                </div>
              </div>

              <!-- Consent -->
              <div class="mb-4">
                <div class="form-check">
                  <input class="form-check-input" type="checkbox" v-model="form.consent" id="consent" required>
                  <label class="form-check-label" for="consent">
                    I confirm that the information provided is accurate to the best of my knowledge and consent to sharing this 
                    report with Healthline purposes.
                  </label>
                </div>
              </div>

              <!-- Submit Buttons -->
              <div class="d-flex gap-3">
                <button type="submit" class="btn btn-success btn-lg" :disabled="isSubmitting" style="background-color: #258576; border-color: #258576;">
                  <span v-if="isSubmitting">
                    <span class="spinner-border spinner-border-sm mr-2" role="status"></span>
                    Submitting...
                  </span>
                  <span v-else>
                    <i class="fas fa-paper-plane mr-2"></i>
                    Submit Report
                  </span>
                </button>
                <button type="button" class="btn btn-outline-secondary btn-lg" @click="resetForm" :disabled="isSubmitting">
                  <i class="fas fa-redo mr-2"></i>
                  Reset Form
                </button>
              </div>

            </form>
          </div>
        </div>
      </div>
    </div>

    <!-- Success Modal -->
    <div class="modal fade" id="successModal" tabindex="-1" aria-labelledby="successModalLabel" aria-hidden="true" ref="successModal">
      <div class="modal-dialog modal-dialog-centered">
        <div class="modal-content border-0 shadow">
          <div class="modal-header border-0 pb-0">
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close" @click="clearForm"></button>
          </div>
          <div class="modal-body text-center px-5 pb-4">
            <div class="mb-3">
              <i class="fas fa-check-circle text-success" style="font-size: 64px;"></i>
            </div>
            <h3 class="mb-3">Report Submitted Successfully!</h3>
            <p class="text-muted mb-2">Reference ID: <strong>#{{ reportReferenceId }}</strong></p>
            <p class="text-muted">Thank you for helping us improve drug safety.</p>
          </div>
          <div class="modal-footer border-0 justify-content-center pt-0 pb-4">
            <button type="button" class="btn btn-success px-4" data-bs-dismiss="modal" @click="clearForm" style="background-color: #258576; border-color: #258576;">
              <i class="fas fa-plus-circle mr-2"></i>
              Submit Another Report
            </button>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import axios from 'axios';
import { Modal } from 'bootstrap';

const BASE_URL = 'http://127.0.0.1:8000';
// const BASE_URL = 'https://prosperc40.pythonanywhere.com';

export default {
  name: 'AdverseReportView',

  data() {
    return {
      isSubmitting: false,
      reportReferenceId: '',
      errorMessage: null,
      bsModal: null,
      form: {
        patient: {
          name: '',
          age: '',
          gender: '',
          weight: ''
        },
        product: {
          drugName: '',
          batchNumber: '',
          expiryDate: '',
          manufacturer: '',
          dosageForm: '',
          doseFrequency: '',
          route: '',
          startDate: '',
          stopDate: ''
        },
        reaction: {
          description: '',
          startDate: '',
          endDate: '',
          severity: '',
          outcome: '',
          seriousness: {
            death: false,
            lifeThreatening: false,
            hospitalization: false,
            disability: false,
            congenital: false,
            other: false
          }
        },
        reporter: {
          name: '',
          type: '',
          phone: '',
          email: ''
        },
        additional: {
          concomitantMeds: '',
          medicalHistory: ''
        },
        consent: false
      }
    };
  },

  mounted() {
    // Initialize Bootstrap modal
    this.bsModal = new Modal(this.$refs.successModal);
  },

  methods: {
    async submitReport() {
      this.isSubmitting = true;
      this.errorMessage = null;

      try {
        const payload = {
          // ── Patient ──────────────────────────────────
          patient_name:   this.form.patient.name   || null,
          patient_age:    this.form.patient.age     || null,
          patient_gender: this.form.patient.gender  || null,
          patient_weight: this.form.patient.weight  || null,

          // ── Product ──────────────────────────────────
          drug_name:          this.form.product.drugName,
          batch_number:       this.form.product.batchNumber   || null,
          expiry_date:        this.form.product.expiryDate    || null,
          manufacturer:       this.form.product.manufacturer  || null,
          dosage_form:        this.form.product.dosageForm    || null,
          dose_frequency:     this.form.product.doseFrequency || null,
          route:              this.form.product.route         || null,
          product_start_date: this.form.product.startDate     || null,
          product_stop_date:  this.form.product.stopDate      || null,

          // ── Reaction ─────────────────────────────────
          reaction_description: this.form.reaction.description,
          reaction_start_date:  this.form.reaction.startDate || null,
          reaction_end_date:    this.form.reaction.endDate   || null,
          severity:             this.form.reaction.severity  || null,
          outcome:              this.form.reaction.outcome   || null,

          // ── Seriousness ──────────────────────────────
          serious_death:            this.form.reaction.seriousness.death,
          serious_life_threatening: this.form.reaction.seriousness.lifeThreatening,
          serious_hospitalization:  this.form.reaction.seriousness.hospitalization,
          serious_disability:       this.form.reaction.seriousness.disability,
          serious_congenital:       this.form.reaction.seriousness.congenital,
          serious_other:            this.form.reaction.seriousness.other,

          // ── Reporter ─────────────────────────────────
          reporter_name:  this.form.reporter.name,
          reporter_type:  this.form.reporter.type,
          reporter_phone: this.form.reporter.phone,
          reporter_email: this.form.reporter.email,

          // ── Additional ───────────────────────────────
          concomitant_meds: this.form.additional.concomitantMeds || null,
          medical_history:  this.form.additional.medicalHistory  || null,
        };

        const response = await axios.post(`${BASE_URL}/api/adr-reports/`, payload);

        // Store the reference ID
        this.reportReferenceId = response.data.report_id || response.data.id || 'N/A';

        // Show success modal
        this.bsModal.show();

      } catch (error) {
        console.error('Submission error:', error);

        if (error.response?.data) {
          this.errorMessage = error.response.data;
        } else {
          this.errorMessage = { 
            error: ['Network error. Please check your connection and try again.'] 
          };
        }

        // Scroll to error message
        this.$nextTick(() => {
          window.scrollTo({ top: 0, behavior: 'smooth' });
        });
      } finally {
        this.isSubmitting = false;
      }
    },

    clearForm() {
      // Reset all form fields
      this.errorMessage = null;
      this.form = {
        patient:  { name: '', age: '', gender: '', weight: '' },
        product:  { drugName: '', batchNumber: '', expiryDate: '', manufacturer: '', dosageForm: '', doseFrequency: '', route: '', startDate: '', stopDate: '' },
        reaction: {
          description: '', startDate: '', endDate: '', severity: '', outcome: '',
          seriousness: { death: false, lifeThreatening: false, hospitalization: false, disability: false, congenital: false, other: false }
        },
        reporter:   { name: '', type: '', phone: '', email: '' },
        additional: { concomitantMeds: '', medicalHistory: '' },
        consent: false
      };

      // Hide modal if it's open
      if (this.bsModal) {
        this.bsModal.hide();
      }

      // Scroll to top
      window.scrollTo({ top: 0, behavior: 'smooth' });
    },

    resetForm() {
      if (confirm('Are you sure you want to reset the form? All entered data will be lost.')) {
        this.clearForm();
      }
    }
  }
};
</script>

<style scoped>
.card-header {
  background-color: #258576 !important;
}

.section-box {
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  padding: 20px;
  background-color: #fafafa;
}

.section-title {
  margin-top: -35px;
  margin-bottom: 20px;
  padding-left: 10px;
}

.section-title i {
  background-color: #fafafa;
  padding: 5px;
}

.form-label {
  font-weight: 500;
}

.text-danger {
  color: #dc3545 !important;
}

.btn-success {
  background-color: #258576 !important;
  border-color: #258576 !important;
}

.btn-success:hover {
  background-color: #1e6e61 !important;
}

.alert-danger {
  border-left: 4px solid #dc3545;
}

/* Modal styles */
.modal-content {
  border-radius: 12px;
}

.modal-header .btn-close {
  font-size: 12px;
}
</style>