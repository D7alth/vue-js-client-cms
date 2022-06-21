<template>
    <div v-if="currentTutoral" class="edit-form">
        <h4>Tutoral</h4>
        <form>
            <div class="form-group">
                <label for="title">title</label>
                <input 
                type="text"
                class="form-control"
                id="title"
                v-model="currentTutoral.title"
                >
            </div>
            <div class="form-group">
                <label for="description">Description</label>
                <input 
                type="text"
                class="form-control"
                id="description"
                v-model="currentTutoral.description"
                >
            </div>
            <div class="form-group">
                <label><strong>Status:</strong></label>
                {{ currentTutoral.published ? "Published" : "Pending" }}
            </div>
        </form>
        <button 
        class="badge badge-primary mr-2"
        v-if="currentTutoral.published"
        @click="updatePublished(false)"
        >
        UnPublish
        </button>
        <button 
        v-else class="badge badge-primary mr-2"      
        @click="updatePublished(true)"
        >
        Publish
        </button>
         <button 
        class="badge badge-primary mr-2"
        @click="deleteTutoral"
        >
        Delete
        </button>
        <button type="submit"
        class="badge badge-success"
        @click="updateTutoral"
        >
        Update
        </button>
        <p>{{ message }}</p>
    </div>
    <div v-else>
        <br>
        <p>please click on a tutoral....</p>
    </div>
</template>

<script>
import TutorialDataService from '../services/TutorialDataService';
/* eslint-disable */
export default {
    name: "tutorial",
    data(){
        return {
            currentTutoral: null,
            message: "",
        };
    },
    methods: {
        getTutorial(id) {
            TutorialDataService.get(id)
                .then(res => {
                    this.currentTutoral = res.data;
                    console.log(res.data);
                }) 
                .catch(e => {
                    console.log(e);
                });
        },
        updatePublished(status) {
            var data = {
                id : this.currentTutoral.id,
                title : this.currentTutoral.title,
                description : this.currentTutoral.description,
                published : status
            };
            TutorialDataService.update(this.currentTutoral.id, data)
                .then(res => {
                    this.currentTutoral.published = status;
                    console.log(res.data);
                })
                .catch(err => {
                    console.log(err);
                });
        },
        updateTutoral() {
            TutorialDataService.update(this.currentTutoral.id, this.currentTutoral)
                .then(res => {
                   console.log(res.data);
                   this.message = "the tutoral was updated successfully";
                })
                .catch(err => {
                    console.log(err);
                });
        },
        deleteTutoral() {
            TutorialDataService.delete(this.currentTutoral.id)
                .then(res => {
                    console.log(res.data);
                    this.$router.push({ name : "tutorals" });
                })
                .catch(err => {
                    console.log(err);
                });
        },
    },
    mounted() {
        this.message = "";
        this.getTutorial(this.$router.params.id);
    },
};
</script>

<style>
.edit-form {
    max-width: 300px;
    margin: auto;
}
</style>