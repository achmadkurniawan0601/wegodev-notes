<template>
    <div class="formNotes">

        <form>
            <div class="menu">
                <button type="button" @click="submitRemove" class="bg-danger btn btn-delete">Delete</button>
                <button type="button" @click="submitSave" class="bg-success btn" v-if="mode == 'save'">Save</button>
                <button type="button" @click="submitUpdate" class="bg-success btn" v-if="mode == 'update'">Update</button>
            </div>

            <div class="content">
                <input type="text" class="text" placeholder="id" v-model="id">
                <input type="text" class="text" placeholder="Title" v-model="title">
                <textarea name="" id="" cols="30" rows="10" class="text textarea" placeholder="Tuliskan Rencana Kamu..." v-model="description"></textarea>
            </div>
        </form>
    </div>
</template>

// boleh mengetik default, untuk mempercepat pengetikan.

<script type="text/javascript">

import axios from 'axios';

export default {
    name: 'formNotes',  
    props: {},
    data: function(){
        return{
            id: 0,
            title: '',
            description: '',
            mode: 'save'
        }
    },
    methods: {
        submitSave(){
            // this.propSaveNote(this.title, this.description);

        let params = new URLSearchParams();
        params.append('title', this.title);
        params.append('description', this.description);

        axios.post('http://localhost/wegodev-notes/note/create', params).then(response => {
            let data = {
                id: response.data.id,
                title: this.title,
                description: this.description
            }
        this.$root.$emit('emitSaveNote', data);
        });
    },
        submitUpdate(){      
            let params = new URLSearchParams();
        params.append('id', this.id);
        params.append('title', this.title);
        params.append('description', this.description);

        axios.post('http://localhost/wegodev-notes/note/update', params).then(response => {
            let data = {
                id: response.data.id,
                title: this.title,
                description: this.description
            }
            this.$root.$emit('emitUpdateNote', data);
        });
    },
        submitRemove(){
            // this.propRemoveNote(this.id); penyebab data tidak terhapus dari list
            let params = new URLSearchParams();
            params.append('id', this.id);

            axios.post('http://localhost/wegodev-notes/note/delete', params).then(response => {
                let data = {
                    id: response.data.id
                }
                this.$root.$emit('emitRemoveNote', data);
                this.resetInput();
            });

        },
        resetInput(){
            this.id = 0;
            this.title = '';
            this. description = '';
        }
    },
    mounted(){
        this.$root.$on('emitForm', data => {
            this.id = data.id;
            this.title = data.title;
            this.description = data.description;
            this.mode = data.mode;
        })
    }
}
</script>

<style scoped>
.menu{
     background: #f7f7f7;
     padding:10px 25px;
     margin-bottom: 25px;
     text-align:right;
     border-bottom: 1px solid #e8e6e6;
}
.btn-delete{ margin-right:10px; }
.content{
     padding: 0px 25px;
}
.text{
     display: block;
     width: 100%;
     padding: 0px;
     font-size: 20px;
     font-weight: bold;
     color: #2c3e50;
     border: none;
     margin-bottom: 10px;
     box-sizing: border-box;
     outline: none;
}
.textarea{
     min-height: 350px;
     font-size: 15px;
     font-weight: lighter;
     line-height: 30px;
}
.loader{
     vertical-align: middle;
}
</style>