<template>
    <div>
        <h1 class="text-center my-5">All Courses</h1>
        <div v-if="message" class="alert alert-success">
            {{message}}
        </div>
        <div class="card-deck" v-for="i in Math.ceil(courses.length / 4)" :key="i">
            <div class="card text-center my-3" v-for="course in courses.slice((i - 1) * 4, i * 4)" v-bind:key="course.id">
                <div class="card-header">
                    <div class="card-title">{{course.username}}</div>
                </div>
                <div class="card-body">
                    <img src="https://placehold.it/150x80?text=IMAGE" class="img-responsive" alt="Image">
                    <div class="card-text">{{course.description}}</div>
                </div>
                <div class="card-footer">
                    <button class="btn btn-outline-danger" v-on:click="deleteCourseClicked(course.id)">Delete
                    </button>
                    <button class="btn btn-outline-success" v-on:click="updateCourseClicked(course.id)">Update
                    </button>
                </div>
            </div>
        </div>
        <div>
            <button class="btn btn-success btn-block my-5" v-on:click="addCourseClicked()">Add new course</button>
        </div>
    </div>
</template>

<script>
    import CourseDataService from "../service/CourseDataService";

    export default {
        name: "CoursesList",
        data() {
            return {
                courses: [],
                message: null,
                INSTRUCTOR: "in28minutes"
            };
        },
        methods: {
            //To make the REST API call, we would need to call the CourseDataService
            refreshCourses() {
                CourseDataService.retrieveAllCourses(this.INSTRUCTOR).then(response => {
                        this.courses = response.data;
                    }
                )
            },
            deleteCourseClicked(id) {
                CourseDataService.deleteCourse(this.INSTRUCTOR, id).then(response => {
                        this.message = response.data + `Delete of course ${id} Successful`;
                        this.refreshCourses();
                    }
                );
            },
            updateCourseClicked(id) {
                this.$router.push(`/courses/${id}`);
            },
            addCourseClicked() {
                this.$router.push(`/courses/-1`);
            }
        },
        //created() - Vue defines a component lifecycle. created will be called as soon as
        // the component is mounted. We are calling refreshCourses as soon
        // as a component is mounted.
        created() {
            this.refreshCourses();
        }
    };
</script>

<style scoped>

</style>