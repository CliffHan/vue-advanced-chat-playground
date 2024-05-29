<template>
  <vue-advanced-chat ref="chatComponent"
    :current-user-id="currentUserId"
    :loading-rooms="loadingRooms"
    :rooms-loaded="roomsLoaded"
    :messages-loaded="messagesLoaded"
    .rooms="rooms"
    .messages="messages"
    .room-actions="roomActions"
    @add-room="addRoom"
    @fetch-messages="fetchMessages($event.detail[0])"
    @send-message="sendMessage($event.detail[0])"
  />
</template>

<script>
import { register } from 'vue-advanced-chat'
register()

// Or if you used CDN import
// window['vue-advanced-chat'].register()

export default {
  data() {
    return {
      currentUserId: '1234',
      loadingRooms: false,
      roomsLoaded: true,
      messagesLoaded: true,
      rooms: [],
      messages: [],
      roomActions: [
        { name: 'inviteUser', title: 'Invite User' },
        { name: 'removeUser', title: 'Remove User' },
        { name: 'deleteRoom', title: 'Delete Room' }
      ]
    }
  },

  mounted() {
    window.chatWindow = this;
  },

  methods: {
    addRoom() {
      console.log('addRoom()');
      let rooms = [...this.rooms];
      rooms.push({
        roomId: '1',
        roomName: 'Room 1',
        users: [
          {
            _id: '1234',
            username: 'John Doe',
          },
          {
            _id: '4321',
            username: 'John Snow',
          }
        ],
      });
      this.rooms = rooms;
    },

    fetchMessages({ room, options }) {
      console.log(`fetchMessages(), room=${JSON.stringify(room)}, options=${JSON.stringify(options)}`);
      this.messagesLoaded = false;
      // use timeout to imitate async server fetched data
      setTimeout(() => {
        this.messages = [];
        this.messagesLoaded = true;
      })
    },

    sendMessage(payload) {
      console.log(`sendMessage(), payload=${JSON.stringify(payload)}`);
      const messages = [...this.messages];
      payload['_id'] = 1;
      payload['senderId'] = '1234';
      messages.push(payload);
      this.messages = messages;
    }
  }
}
</script>