<template>
  <div class="list row">
    <div class="col-md-8">
      <div class="input-group mb-3">
        <input type="text" class="form-control" placeholder="Search by Tech Type" v-model="techType" />
        <div class="input-group-append">
          <button class="btn btn-outline-secondary" type="button" @click="searchTechType">
            Search
          </button>
        </div>
      </div>
    </div>
    <div class="col-md-6">
      <div class="card-header card-header-separator">
        <div>
          <h5 class="mb-0">Tech Items</h5>
        </div>
        <button @click="onAddButton">Register New Tech</button>
      </div>
      <div class="table-responsive">
        <table id="datatable-basic" class="table table-flush">
        <thead class="thead-light">
          <tr>
            <!-- Action Column Heading -->
            <th class="text-uppercase text-secondary text-xxs font-weight-bolder opacity-7">Action</th>
            <!-- Tech Type Column Heading -->
            <th class="text-uppercase text-secondary text-xxs font-weight-bolder opacity-7">Type</th>
            <!-- Name Column Heading -->
            <th class="text-uppercase text-secondary text-xxs font-weight-bolder opacity-7">Name</th>
            <!-- Amount Column Heading -->
            <th class="text-uppercase text-secondary text-xxs font-weight-bolder opacity-7">Amount</th>
          </tr>
        </thead>
        <tbody>
          <!-- <tr :class="{ active: index == currentIndex }" v-for="(techItem, index) in techItems"
            :key="index" @click="setActiveTechItem(techItem, index)"> -->
            <tr :class="{ active: index == currentIndex }" v-for="(techItem, index) in techItems"
            :key="index">
            <!-- Edit & Delete function icon -->
            <td class="text-sm font-weight-normal">
              <i class="material-icons-round opacity-10 pointer" @click="onEditButton(techItem)">edit</i>
              <i class="material-icons-round opacity-10 pointer" @click="onDeleteButton(techItem)">delete</i>
            </td>
            <td class="text-sm font-weight-normal">{{ techItem.techType }}</td>
            <td class="text-sm font-weight-normal">{{ techItem.techName }}</td>
            <td class="text-sm font-weight-normal">{{ techItem.amount }}</td>
          </tr>
        </tbody>
      </table>
      </div>

      <!-- <button class="m-3 btn btn-sm btn-danger" @click="removeAllTechItems">
        Remove All
      </button> -->
    </div>
    <!-- <div class="col-md-6">
      <div v-if="currentTechItem">
        <h4>Tech Item</h4>
        <div>
          <label><strong>Tech Type:</strong></label> {{ currentTechItem.techType }}
        </div>
        <div>
          <label><strong>Tech Name:</strong></label> {{ currentTechItem.techName }}
        </div>
        <div>
          <label><strong>Amount:</strong></label> {{ currentTechItem.amount }}
        </div>

        <router-link :to="'/techItems/' + currentTechItem.id" class="badge badge-warning">Edit</router-link>
      </div>
      <div v-else>
        <br />
        <p>Please click on a Tech Item...</p>
      </div>
    </div> -->
  </div>
</template>

<script>
import TechDataService from "../services/TechDataService";

export default {
  name: "tech-item-list",
  data() {
    return {
      techItems: [],
      currentTechItem: null,
      currentIndex: -1,
      techType: ""
    };
  },
  methods: {
    retrieveTechItems() {
      TechDataService.getAll()
        .then(response => {
          this.techItems = response.data;
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    },

    refreshList() {
      this.retrieveTechItems();
      this.currentTechItem = null;
      this.currentIndex = -1;
    },

    // setActiveTechItem(techItem, index) {
    //   this.currentTechItem = techItem;
    //   this.currentIndex = techItem ? index : -1;
    // },

    onEditButton(techItem) {
      this.currentTechItem = techItem;
      this.$router.push({ name: 'techItem-details', params: { id: this.currentTechItem.id } });
    },

    // removeAllTechItems() {
    //   TechDataService.deleteAll()
    //     .then(response => {
    //       console.log(response.data);
    //       this.refreshList();
    //     })
    //     .catch(e => {
    //       console.log(e);
    //     });
    // },

    searchTechType() {
      TechDataService.findByTechType(this.techType)
        .then(response => {
          this.techItems = response.data;
          this.setActiveTechItem(null);
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    },

    onAddButton() {
      this.$router.push({ name: 'add' });
    },

    onDeleteButton(techItem) {
      this.currentTechItem = techItem;
      // this.$swal
      //   .fire({
      //     title: 'Are you sure?',
      //     icon: 'warning',
      //     showConfirmButton: true,
      //     showCancelButton: true,
      //   }).then((res) => {
      //     if (res.isConfirmed === true) {
      TechDataService.delete(this.currentTechItem.id)
        .then(response => {
          console.log(response.data);
          // this.$swal.fire({
          //   title: 'Deleted',
          //   position: 'top',
          //   icon: 'success',
          //   toast: true,
          //   showConfirmButton: false,
          //   timer: 2000,
          //   timerProgressBar: true,
          // });
          this.refreshList();
        })
        .catch(e => {
          console.log(e);
        });
      // }
      // });
    }
  },
  mounted() {
    this.retrieveTechItems();
  }
};
</script>

<style>
.list {
  text-align: left;
  max-width: 750px;
  margin: auto;
}
</style>