<template>
  <!-- Modal Add-->
    <!-- Modal content-->
        <div class='modal-content'>
          <div class='modal-header'>
            <h4 class='modal-title'>Input Task</h4>
          </div>
          <div class='modal-body'>
            <form>
              <div class='form-group'>
                <label for='Title'>Title:</label>
                <input v-model='inputTask.title' type='text' class='form-control' id='title'>
              </div>
              <div class='form-group'>
                <label for='Description'>Description:</label>
                <textarea v-model='inputTask.desc' class='form-control' rows='5' id='description'></textarea>
              </div>
              <div class='form-group'>
                <label for='Point'>Point:</label>
                <input v-model='inputTask.point' type='text' class='form-control' id='point'>
              </div>
              <div class='form-group'>
                <label for='AssignTo'>Assign To:</label>
                <input v-model='inputTask.assignTo' type='text' class='form-control' id='assign'>
              </div>
            </form>
          </div>
          <div class='modal-footer'>
            <button type='button' class='btn btn-default' data-dismiss='modal'>Close</button>
            <button @click='saveTask' type='button' class='btn btn-default' data-dismiss='modal'>Add Task</button>
          </div>
        </div>

</template>

<script>
export default {

}
</script>

<style>

</style>
