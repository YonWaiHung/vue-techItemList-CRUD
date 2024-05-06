<template>
  <form v-if="currentTechItem" @submit.prevent="onSave">
    <div class="py-4 container-fluid">
      <!-- Upper Row Section -->
      <div class="row">
        <div class="col-lg-6">
          <h4>Editing Tech Item</h4>
        </div>
        <div
          class="text-right col-lg-6 d-flex flex-column justify-content-center"
        >
          <div class="d-flex ms-lg-auto me-lg-0 mt-lg-0">
            <button type="submit" class="mt-2 mb-0 btn bg-gradient-success" @click="updateTechItem">
              Save
            </button>
          </div>
        </div>
      </div>
      <div class="mt-4 row">
        <div class="mt-4 col-lg-12 mt-lg-0">
          <div class="card">
            <div class="card-body mb-3">
              <h5 class="font-weight-bolder">Tech Item Information</h5>
              <div class="row mt-4">
                <div class="col-12 col-sm-6">
                  <label class="form-label select">Tech Type</label>
                  <div class="custom-dropdown mt-2">
                    <input type="text" class="form-control" id="techType"
                      v-model="currentTechItem.techType"
                    />
                  </div>
                </div>
                <div class="col-12 col-sm-6">
                  <label class="form-label select">Tech Name</label>
                  <div class="custom-dropdown mt-2">
                    <input type="text" class="form-control" id="techName"
                      v-model="currentTechItem.techName"
                    />
                  </div>
                </div>
                <div class="col-12 col-sm-6">
                  <label class="form-label select">Amount</label>
                  <div class="custom-dropdown mt-2">
                    <input type="number" class="form-control" id="amount"
                      v-model="currentTechItem.amount"
                    />
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </form>
  <!-- <div v-if="currentTechItem" class="edit-form">
    <h4>Tech Item</h4>
    <form>
      <div class="form-group">
        <label for="techType">Tech Type</label>
        <input type="text" class="form-control" id="techType"
          v-model="currentTechItem.techType"
        />
      </div>
      <div class="form-group">
        <label for="techName">Tech Name</label>
        <input type="text" class="form-control" id="techName"
          v-model="currentTechItem.techName"
        />
      </div>

      <div class="form-group">
        <label for="amount">Amount</label>
        <input type="number" class="form-control" id="amount"
          v-model="currentTechItem.amount"
        />
      </div>
    </form>

    <button class="badge badge-danger mr-2"
      @click="deleteTechItem"
    >
      Delete
    </button>

    <button type="submit" class="badge badge-success"
      @click="updateTechItem"
    >
      Update
    </button>
    <p>{{ message }}</p>
  </div> -->

  <div v-else>
    <br />
    <p>Please click on a Tech Item...</p>
  </div>
</template>

<script>
import TechDataService from "../services/TechDataService";

export default {
  name: "tech-item",
  data() {
    return {
      currentTechItem: null,
      message: ''
    };
  },
  methods: {
    getTechItem(id) {
      TechDataService.get(id)
        .then(response => {
          this.currentTechItem = response.data;
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    },

    updateTechItem() {
      TechDataService.update(this.currentTechItem.id, this.currentTechItem)
        .then(response => {
          if (response.status === 200 || response.status === 201) {
          //   console.log(response.data);
          //   this.message = 'The tech item was updated successfully!';
          //   this.$swal.fire({
          //     title: 'Saved',
          //     position: 'top',
          //     icon: 'success',
          //     toast: true,
          //     showConfirmButton: false,
          //     timer: 2000,
          //     timerProgressBar: true,
          // });
          this.$router.push({ name: 'techItems' });
          }
        })
        .catch(e => {
          console.log(e);
        });
    },

    // deleteTechItem() {
    //   TechDataService.delete(this.currentTechItem.id)
    //     .then(response => {
    //       console.log(response.data);
    //       this.$router.push({ name: "techItems" });
    //     })
    //     .catch(e => {
    //       console.log(e);
    //     });
    // }
  },
  async mounted() {
    this.message = '';
    this.getTechItem(this.$route.params.id);
  }
};
</script>

<style>
.edit-form {
  max-width: 300px;
  margin: auto;
}
</style>