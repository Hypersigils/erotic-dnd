<template>
  <div class="message">
		<span v-html="richTexted" />
		<span class="editMessage" v-if="isEdited">(edited)</span>
	</div>
</template>

<script>
export default {
	props: [ "message", "isEdited" ],
	computed: {
		richTexted() {
			let richMessage = this.message;
			
			while (richMessage.match(/_(.*)_/) || richMessage.match(/\*\*(.*)\*\*/)) {
				richMessage = richMessage.replace(/_(.*?)_/, "<span style='font-style: italic;'>$1</span>");
				
				richMessage = richMessage.replace(/\*\*(.*?)\*\*/, "<span style='font-weight: bold;'>$1</span>");
			}

			return richMessage;
		}
	}
}
</script>

<style>
.editMessage {
	font-size: 0.6em;
	margin-left: 5px;
	color: #72767d;
}
</style>