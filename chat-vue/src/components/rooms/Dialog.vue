<template>
    <RoomSlot>
        <mu-col style="width: 100%;" align-items="end">
            <AddUsers :room="id"></AddUsers>
            <mu-container class="dialog">
                <mu-row direction="column"
                        justify-content="start"
                        align-items="end"
                        v-for="dialog in dialogs">
                    <p><strong>{{dialog.user.username}}</strong></p>
                    <p>{{dialog.text}}</p>
                    <span>{{dialog.date}}</span>
                </mu-row>
            </mu-container>
            <mu-container>
                <mu-row justify-content="end" align-items="end">
                    <mu-text-field v-model="form.textarea"
                                   multi-line
                                   :rows="3"
                                   full-width
                                   placeholder="Введите текст сообщения">
                    </mu-text-field>
                    <mu-button class="btn-send" round color="success" @click="sendMes">Отправить</mu-button>
                </mu-row>
            </mu-container>
            <br>
        </mu-col>
    </RoomSlot>
</template>

<script>
    import RoomSlot from './Room'
    import AddUsers from './AddUsers'

    export default{
        name: "Dialog",
        props: {
            id: '',
        },
        components: {
            AddUsers,
            RoomSlot,
        },
        data() {
            return {
                dialogs: '',
                form: {
                    textarea: '',
                },
            }
        },
        mounted() {
            this.loadDialog()
        },
        created() {
            $.ajaxSetup({
                headers: {'Authorization': 'Token ' + sessionStorage.getItem("auth_token")},
            });
            this.loadDialog()
            setInterval(() => {
                this.loadDialog()
            }, 5000)
        },
        methods: {
            loadDialog() {
                $.ajax({
                    url: "http://127.0.0.1:8000/api/v1/chat/dialog/",
                    type: "GET",
                    data: {
                        room: this.$route.params.id
                    },
                    success: (response) => {
                        this.dialogs = response.data.data
                    }
                });
            },
            sendMes() {
                $.ajax({
                    url: "http://127.0.0.1:8000/api/v1/chat/dialog/",
                    type: "POST",
                    data: {
                        room: this.$route.params.id,
                        text: this.form.textarea
                    },
                    success: (response) => {
                        this.loadDialog()
                    },
                    error: (response) => {
                        alert(response.status.text)
                    }
                });
            }
        },
    }
</script>

<style scoped>
    .dialog {
        border: 1px solid #000;
    }
</style>
