<template>
  <div class="sc-message-list" ref="scrollList">
    <Message v-for="(message, idx) in messages" :message="message" :chatImageUrl="chatImageUrl(message.author)" :authorName="authorName(message.author)" :key="idx" :colors="colors" :messageStyling="messageStyling" />
    <Message v-show="showTypingIndicator !== ''" :message="{author: showTypingIndicator, type: 'typing'}" :chatImageUrl="chatImageUrl(showTypingIndicator)" :colors="colors" :messageStyling="messageStyling" />
  </div>
</template>
<script>
import Message from './Message.vue'
import chatIcon from './assets/chat-icon.svg'

export default {
  components: {
    Message
  },
  props: {
    participants: {
      type: Array,
      required: true
    },
    messages: {
      type: Array,
      required: true
    },
    showTypingIndicator: {
      type: String,
      required: true
    },
    colors: {
      type: Object,
      required: true
    },
    alwaysScrollToBottom: {
      type: Boolean,
      required: true
    },
    messageStyling: {
      type: Boolean,
      required: true
    }
  },
  methods: {
    _scrollDown () {
      this.$refs.scrollList.scrollTop = this.$refs.scrollList.scrollHeight
    },
    shouldScrollToBottom () {
      return this.alwaysScrollToBottom || (this.$refs.scrollList.scrollTop > this.$refs.scrollList.scrollHeight - 600)
    },
    profile (author) {
      const profile = this.participants.find(profile => profile.id === author)

      // A profile may not be found for system messages or messages by 'me'
      return profile || {imageUrl: '', name: ''}
    },
    chatImageUrl (author) {
      return this.profile(author).imageUrl
    },
    authorName (author) {
      return this.profile(author).name
    }
  },
  computed: {
    defaultChatIcon () {
      return chatIcon
    }
  },
  mounted () {
    this._scrollDown()
  },
  updated () {
    if (this.shouldScrollToBottom()) { this.$nextTick(this._scrollDown()) }
  }
}
</script>

<style scoped>
.sc-message-list {
  height: 100%;
  align-content: center;
  overflow-y:auto;
  background-size: contain;
  background-repeat: no-repeat;
  padding: 40px 0px;
  background-image: url('https://ur.columbusstate.edu/logos/logo_files/2015/CSU_Logo_Tower.png');
  background-color: rgba(255,255,255,0.75);
  background-blend-mode: lighten;
}
</style>
