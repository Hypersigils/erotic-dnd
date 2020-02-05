<template>
	<div>
		<div class="messageBox">
			<div class="messageIcon" style="background-color: red;">
				<img src=""/>
			</div>
			<div class="messages">
				<div class="header">
					<span class="username">{{ log.user }}</span>
					<span class="time">{{ timeStamp }}</span>
				</div>
				<Message v-for="(message, index) of log.messages" 
					:key="message" 
					:message="message" 
					:isEdited="log.edits && log.edits.includes(index)" />
			</div>
			<v-divider class="divider" v-if="hasDivider" />
		</div>
		<div class="messageBox" v-if="log.roll">
			<div class="messageIcon" style="background-color: red;">
				<img src=""/>
			</div>
			<div class="messages">
				<div class="header">
					<span class="username">Wheel</span>
					<span class="time">{{ timeStamp }}</span>
				</div>
				<Message v-for="message of [`**${log.user}** is rolling _${log.roll.stat}._`, `1d20`]" :key="message" :message="message" />
			</div>
			<v-divider class="divider" v-if="hasDivider" />
		</div>
	</div>
</template>

<script>
import Message from '@/components/Message'

export default {
	props: [ "logs", "log", "hasDivider", "index", "startingMinutes" ],
	components: {
		Message
	},
	computed: {
		timeStamp() {
			let currentMinutes = this.startingMinutes;
			for (let i=0; i<this.index && i<this.logs.length; i++) {
				if (this.logs[i].timestamp) currentMinutes += this.logs[i].timestamp;
			}

			let hours = Math.floor(currentMinutes / 60);
			let minutes = Math.floor(currentMinutes - (hours * 60));
			let m = "AM";
			if (hours >= 12) {
				if (hours > 12) hours -= 12;
				m = "PM";
			}
			if (minutes > 10) return `${hours}:${minutes} ${m}`;
			return `${hours}:0${minutes} ${m}`;
		}
	}
}
</script>

<style scoped>
.messageBox {
    display: grid;
    grid-template-columns: 40px 1fr;
    grid-template-rows: 1fr 2px;
    grid-column-gap: 20px;
    grid-row-gap: 20px;

    padding-left: 20px !important;
    padding-top: 20px !important;
    /* padding-bottom: 20px !important; */
}

.messageIcon {
    height: 40px;
    width: 100%;

    border-radius: 50%;
}

.messages {
    margin-top: -5px !important;
}

.username {
    font-weight: bold;
    margin-right: 10px;

    color: white;
}

.time {
    color: #72767d;

    font-size: 0.75rem;
    font-weight: 500;
}

.divider {
    width: 100%;
    grid-column-start: 1;
    grid-column-end: 3;
    background-color: hsla(0, 0%, 100%, 0.1);
}

/* Font: Whitney,Helvetica Neue,Helvetica,Arial,sans-serif */
</style>