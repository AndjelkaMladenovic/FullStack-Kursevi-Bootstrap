<template>
    <div>
       <h1>Instructor Aplications!</h1><br/>
        <ListCoursesComponent/>
    </div>
</template>

<script>
    import ListCoursesComponent from "./ListCoursesComponent";
    export default {
        name: "InstructorApp",
        components: {
            ListCoursesComponent
        }
    }
</script>

<style scoped>

</style>
