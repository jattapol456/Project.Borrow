<template lang="pug">
body
    .pt-14.pl-20
        .wrapper.pl-6
            form(@submit.prevent="addItems")
                .pl-16.pt-10.space-y-10
                    div
                        div(v-if="!image").space-y-4
                            h2 Select an image
                            input(type="file" @change="onFileChange" :required="true")
                        div(v-else).flex.items-end.space-x-4
                            img(:src="image")
                            button.text-red-main.pb-1(@click="removeImage") Remove image
                    .pr-96.space-y-10
                        .flex.justify-end.items-center.space-x-4.pr-80
                            p Item :
                            input(class="input" type="text" id="item" v-model="name" :required="true").py-2.px-4
                        .flex.justify-end.items-center.space-x-4.pr-80
                            p Brand :
                            input(class="input" id="brand" v-model="brand" :required="true")
                        .flex.justify-end.items-center.space-x-4.pr-80
                            p Model :
                            input(class="input" id="model" v-model="model" :required="true").py-2.px-4
                        .flex.justify-end.items-center.space-x-4.pr-80
                            p Code IP :
                            input(class="input" id="code_ip" v-model="code_ip" :required="true").py-2.px-4
                        .flex.justify-end.items-center.space-x-4.pr-80
                            p Status :
                            select(v-model="statusitem" :required="true").text-gray-700.py-2.px-4.rounded.inline-flex.items-center.shadow.border-2
                                option AVAILABLE
                                option UNAVAILABLE

                .flex.justify-center.pt-10
                    button(type="submit")
                        Buttonred.w-60.h-10 ADD DIVICE
                
</template>

<script lang="js">
import Buttonred from '../../components/Buttonred.vue'

export default {
	components: {
        Buttonred,
    },
    layout: "layoutadmin",
    data: () => {
        return {
            files: "",
            image: "",
            name: "",
            brand: "",
            model: "",
            code_ip: "",
            statusitem: "",
        }
    },
    methods: {
        onFileChange(e) {
            const files = e.target.files || e.dataTransfer.files;
            if (!files.length) 
                return;
            this.createImage(files[0]);
            this.files=files[0]
        },
        createImage(file) {
            const reader = new FileReader();
            const vm = this;

            reader.onload = (e) => {
                vm.image = e.target.result;
            };
            reader.readAsDataURL(file);
        },
        removeImage (e) {
            this.image = "";
        },
        async addItems() {
            const ref = this.$fire.storage.ref().child(`images/${this.files.name}`);
            await ref.put(this.files)
            const url = await ref.getDownloadURL()
            this.$store.dispatch("addItems", {
                img:url,
                name:this.name,
                brand:this.brand,
                model:this.model,
                code_ip:this.code_ip,
                statusitem:this.statusitem
            })
            location.reload()
        }
    }
}
</script>

<style lang="scss" scoped>
.wrapper {	
	margin-top: 20px;
    margin-bottom: 80px;
}
form {
    background: #FFFFFF;
    padding-bottom: 45px;
    width: 1100px;
    box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.2), 0 5px 5px 0 rgba(0, 0, 0, 0.24);
}
img {
  width: 200px;
  display: block;
  margin-bottom: 10px;
}
select {
    width: 217px;
}
</style>