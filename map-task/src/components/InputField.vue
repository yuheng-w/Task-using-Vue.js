<template>
    <br>
    <form>
        <div class="form-inline">
            <div class="col-5">
                <input
                    type="text"
                    v-model="address"
                    class="form-control mb-2" 
                />
            </div>
            <div class="col-auto">
                <button
                    class="btn btn-primary mb-2"
                    @click.prevent="submitFunction(address)"
                > Submit </button>
            </div>
        </div>
    </form>
</template>

<script>
export default {
    props: ['submitFunction'],
    data() {
        return {
            address: ''
        }
    }
}
</script>

<style>
  .inline-elements {
    display: inline;
  }
</style>