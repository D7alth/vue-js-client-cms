<template>
    <div class="list row">  
        <div class="col-md-8">
            <div class="input-group mb-3">
                <input 
                type="text"
                class="form-control"
                placeholder="Search by title"
                v-model="title"
                >
                <div class="input-group-append">
                    <button 
                    class="btn btn-outline-secondary"
                    type="button"
                    @click="seatchTitle"
                    >
                    Search
                    </button>
                </div>
            </div>
        </div> 
        <div class="col-md-6">
            <h4>Tutorials List</h4>
            <ul class="list-group">
                <li 
                class="list-group-item"
                :class="{active: index == currentIndex}"
                v-for="(tutorial, index) in tutorials"
                :key="index"
                @click="setActiveTutorial(tutorial, index)" 
                >
                {{ tutorial.title }}
                </li>
            </ul>
            <button 
            class="m-3 btn btn-sm btn-danger" 
            @click="removeAllTutorials">
            Remove all
            </button>
        </div>       
        <div class="col-md-6">
            <div v-if="currentTutoral">
                <h4>Tutoral</h4>
                <div>
                    <label><strong>Title: </strong></label> {{ currentTutoral.title }}
                </div>
                <div>
                    <label><strong>Description: </strong></label> {{ currentTutoral.description }}
                </div>
                <div>
                    <label><strong>Status: </strong></label> {{ currentTutoral.status }}
                </div>
                <a 
                class="badge badge-warning" 
                href="'/tutorials/' + currentTutoral.id">
                edit
                </a>
            </div>
            <div v-else>
                <br>
                <p>Plase click on a tutoral...</p>
            </div>
        </div>
    </div>
</template>

<script>
import TutorialDataService from '../services/TutorialDataService';
export default {
    name: "tutorial-list",
    data() {
        return {
            tutorials: [],
            currentTutoral: null,
            currentIndex: -1,
            title:""
        }
    },
    methods: {
        retriveTutorials() {
            TutorialDataService.getAll()   
                .then(res => {
                    this.tutorials = res.data;
                    console.log(res.data);
                })
                .catch(err => {
                    console.log(err);
                });
        },
        refreshList() {
            this.retriveTutorials();
            this.retriveTutorials = null;
            this.currentTutoral = -1;
        },
        setActiveTutorial(tutorial, index) {
            this.currentTutoral = tutorial;
            this.currentIndex = index;
        },
        removeAllTutorials() {
            TutorialDataService.deleteAll()
                .then(res => {
                    console.log(res);
                    this.refreshList();
                })
                .catch(err => {
                    console.log(err);
                })
        },
        searchTitle() {
            TutorialDataService.findByTitle(this.title)
                .then(res => {
                    this.tutorials = res.data;
                    console.log(res.data);
                })
                .catch(err => {
                    console.log(err);
                })
        }
    },
    mounted() {
        this.retriveTutorials();
    }
    
}
</script>
<style>
.list {
    text-align: left;
    max-width: 750px;
    margin: auto;
}
</style>