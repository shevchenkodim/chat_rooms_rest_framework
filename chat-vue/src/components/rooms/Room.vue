<template>
    <HomeSlot>
        <mu-row>
            <mu-col span="3" style="width: 100%;" class="rooms-list">
                <mu-button @click="addRoom">Создать комнату</mu-button>
                <div v-for="room in rooms">
                    <h3 @click="openDialog(room.id)">{{room.creater.username}}</h3>
                    <small>{{room.date}}</small>
                </div>
            </mu-col>
            <slot></slot>
        </mu-row>
    </HomeSlot>
</template>

<script>
    import HomeSlot from '../Home'

    export default{
        name: "Room",
        components: {
            HomeSlot,
        },
        data() {
            return {
                rooms: '',
            }
        },
        created() {
            $.ajaxSetup({
                headers: {'Authorization': 'Token ' + sessionStorage.getItem("auth_token")},
            });
            this.loadRoom()
        },
        methods: {
            loadRoom() {
                $.ajax({
                    url: 'http://127.0.0.1:8000/api/v1/chat/room/',
                    type: 'GET',
                    success: (response) => {
                        this.rooms = response.data.data
                    }
                });
            },
            openDialog(id) {
                //this.$emit("openDialog", id)
                this.$router.push({name: 'dialog', params: {id: id}})
            },
            // Создание комнаты
            addRoom() {
                $.ajax({
                    url: "http://127.0.0.1:8000/api/v1/chat/room/",
                    type: "POST",
                    success: (response) => {
                        this.loadRoom()
                    },
                    error: (response) => {
                        alert(response.statusText)
                    }
                })
            }
        }
    }
</script>

<style scoped>
    h3 {
        cursor: pointer;
    }

    .rooms-list {
        margin: 0 10px 0 0;
        padding: 1 0 1 1;
        box-shadow: 1px 4px 5px #848181;
    }
</style>
