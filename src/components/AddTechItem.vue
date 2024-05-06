<template>
  <form @submit.prevent="onSave">
    <div class="py-4 container-fluid">
      <div class="row">
        <div class="col-lg-6">
          <h4>New Tech Item</h4>
        </div>
        <div
          class="text-right col-lg-6 d-flex flex-column justify-content-center"
        >
          <div class="d-flex ms-lg-auto me-lg-0 mt-lg-0">
            <button @click="saveTechItem" class="mt-2 mb-0 btn bg-gradient-success" type="submit">
              Save
            </button>
          </div>
        </div>
      </div>
    </div>
  </form>
  <div class="mt-4 row">
    <div class="mt-4 col-lg-12 mt-lg-0">
      <div class="card">
        <div class="card-body mb-3">
          <h5 class="font-weight-bolder">Tech Information</h5>
          <div class="row mt-4">
            <div class="col-12 col-sm-6">
              <label class="form-label select">Tech Type</label>
              <div class="custom-dropdown mt-2">
                <input
                  type="text"
                  class="form-control"
                  id="techType"
                  required
                  v-model="techItem.techType"
                  name="techType"
                />
              </div>
            </div>
            <div class="col-12 col-sm-6">
              <label class="form-label select">Tech Name</label>
              <input
                class="form-control"
                id="techName"
                required
                v-model="techItem.techName"
                name="techName"
              />
            </div>
            <div class="col-12 col-sm-6">
              <label for="amount">Amount</label>
              <input
                class="form-control"
                type="number"
                id="amount"
                required
                v-model="techItem.amount"
                name="amount"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <!-- <div class="submit-form">
    <div v-if="!submitted">
      <div class="form-group">
        <label for="techType">Tech Type</label>
        <input
          type="text"
          class="form-control"
          id="techType"
          required
          v-model="techItem.techType"
          name="techType"
        />
      </div>

      <div class="form-group">
        <label for="techName">Tech Name</label>
        <input
          class="form-control"
          id="techName"
          required
          v-model="techItem.techName"
          name="techName"
        />
      </div>

      <div class="form-group">
        <label for="amount">Amount</label>
        <input
          class="form-control"
          type="number"
          id="amount"
          required
          v-model="techItem.amount"
          name="amount"
        />
      </div>

      <button @click="saveTechItem" class="btn btn-success">Submit</button>
    </div>

    <div v-else>
      <h4>You submitted successfully!</h4>
      <button class="btn btn-success" @click="newTechItem">Add</button>
    </div>
  </div> -->
</template>

<script>
import TechDataService from "../services/TechDataService";

export default {
  name: "add-tech-item",
  data() {
    return {
      techItem: {
        id: null,
        techType: "",
        techName: "",
        amount: 0,
      },
      submitted: false
    };
  },
  methods: {
    saveTechItem() {
      var data = {
        techType: this.techItem.techType,
        techName: this.techItem.techName,
        amount: this.techItem.amount
      };

      TechDataService.create(data)
        .then(response => {
          if (response.status === 200 || response.status === 201) {
            // this.techItem.id = response.data.id;
            // console.log(response.data);
            this.submitted = true;
            // this.$swal.fire({
            //     title: 'Item Registered Successfully!',
            //     position: 'top',
            //     icon: 'success',
            //     toast: true,
            //     showConfirmButton: false,
            //     timer: 2000,
            //     timerProgressBar: true,
            //   });
            this.$router.push({ name: 'techItems' });
            }
        })
        .catch(e => {
          console.log(e);
        });
    },
    
    newTechItem() {
      this.submitted = false;
      this.techItem = {};
    }
  }
};
</script>

<style>
.submit-form {
  max-width: 300px;
  margin: auto;
}
</style>