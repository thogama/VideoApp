<template>
    <div class="col-4">
        <div class=" fs-5 bg-info text-center rounded p-2">
            This Component has a script that connect to signaling's server

        </div>
        <div class="my-3">
            <label for="key" class="form-label">Friend's key</label>


            <div class="input-group mb-3">
                <span class="input-group-text" id="basic-addon1">
                    <Icon name="grommet-icons:key" />
                </span>
                <input v-model="key" type="text" class="form-control" placeholder="key" aria-label="key"
                    aria-describedby="basic-addon1">


            </div>
            <div class="input-group mb-3">
                <input readonly type="text" class="form-control" placeholder="Your generated key" aria-label="Your generated key"
                    aria-describedby="basic-addon2">
                <span class=" border  input-group-text" id="basic-addon2">
                    <div class=" btn p-0">
                        <Icon class="m-1" name="mingcute:copy-2-fill" />

                    </div>
                </span>
            </div>
            <div class="d-flex  align-items-center  justify-content-evenly">

                <div @click="generateKey()" class="btn btn-warning text-white">
                    Generate key
                </div>
                or
                <div @click="makeConnection(key)" class="btn btn-warning text-white">
                    Start chat
                </div>

            </div>

        </div>
    </div>
</template>

<script lang="ts" setup>
let key = ref("")
function makeConnection(key: string) {
    console.log(key)
    let container = document.getElementById("video-container")
    let video = document.getElementById("video-tag")!.cloneNode()
    container?.append(video)
}
function generateKey() {

}


</script>

<style lang="scss" scoped></style>