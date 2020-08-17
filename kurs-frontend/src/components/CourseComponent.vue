<template>
    <div class="container">
        <div class="card mt-5">
            <div class="card-header text-center">
                <h1>New course</h1>
            </div>
            <form class="card-body" @submit="validateAndSubmit">
                <div v-if="errors.length">
                    <div class="alert alert-warning" v-bind:key="index" v-for="(error, index) in errors">{{error}}</div>
                </div>
                <fieldset class="form-group card-text">
                    <label>Name</label>
                    <input type="text" class="form-control" v-model="username">
                </fieldset>
                <fieldset class="form-group card-text">
                    <label>Description</label>
                    <input type="text" class="form-control" v-model="description">
                </fieldset>
                <button class="btn btn-success btn-block" type="submit">Save</button>
            </form>
        </div>
    </div>
</template>

<script>
    import CourseDataService from "../service/CourseDataService";

    export default {
        name: "courseDetails",
        data() {
            return {
                username: "",
                description: "",
                INSTRUCTOR: "in28minutes",
                errors: []
            };
        },
        computed: {
            id() {
                return this.$route.params.id;
            }
        },
        methods: {
            refreshCourseDetails() {
                CourseDataService.retrieveCourse(this.INSTRUCTOR, this.id).then(res => {
                    this.username = res.data.username;
                    this.description = res.data.description;
                });
            },
            validateAndSubmit(e) {
                e.preventDefault();
                this.errors = [];
                if (!this.username) {
                    this.errors.push("Enter course name!");
                } else if (this.username.length < 1) {
                    this.errors.push("Enter atleast 1 characters in Username");
                }
                if (!this.description) {
                    this.errors.push("Enter description!");
                } else if (this.description.length < 5) {
                    this.errors.push("Enter atleast 5 characters in Description");
                }

                if (this.errors.length === 0) {
                    if (this.id === -1) {
                        CourseDataService.createCourse(this.INSTRUCTOR, {
                            username: this.username,
                            description: this.description
                        }).then(() => {
                            this.$router.push("/courses");
                        });
                    } else {
                        CourseDataService.updateCourse(this.INSTRUCTOR, this.id, {
                            id: this.id,
                            username: this.username,
                            description: this.description
                        }).then(() => {
                            this.$router.push("/courses");
                        });
                    }
                }
            }
        },
        created() {
            this.refreshCourseDetails();
        }
    };
</script>

<style scoped>

</style>