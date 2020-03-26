<template>
    <div class="rooms">
        <div>
            <ul>
                <li v-for="room in rooms">
                    <h3 @click="openDialog(room.id)">{{room.creater.username}}</h3>
                    <small>{{room.date}}</small>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
    import $ from 'jquery'

    export default{
        name: "Room",
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
                this.$emit("openDialog", id)
            },
        }
    }
</script>

<style scoped>
    .rooms {
        width: 15%;
        height: 100%;
    }
    h3 {
        cursor: pointer;
    }
</style>
