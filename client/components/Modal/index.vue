<template>
  <div class="modal-wrapper" :style="{ '--width': width, '--overflow': overflow, '--min-height': minHeight }">
    <div v-click-outside="close" v-shortkey="['esc']" class="modal-content" @shortkey="close">
      <Icon name="close" class="close-icon" @click.native="close" />
      <slot></slot>
    </div>
  </div>
</template>

<script>
import ClickOutside from 'vue-click-outside'

export default {
	directives: {
		ClickOutside
	},
	props: {
		width: {
			type: String,
			default: '900px'
		},
		overflow: {
			type: String,
			default: 'auto'
		},
		minHeight: {
			type: String,
			default: undefined
		},
		canClose: {
			type: Boolean,
			default: true
		}
	},
	methods: {
		close(e) {
			if (!this.canClose) return

			e.stopPropagation()

			// Prevent close click from triggering on another element below the modal
			document.body.classList.add('no-events')

			this.$emit('close')

			// After the delay, allow events to trigger again
			setTimeout(() => {
				document.body.classList.remove('no-events')
			}, 700)
		}
	}
}

</script>

<style lang="scss" scoped>
	.modal-wrapper {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		min-height: 100vh;
		background: rgba(0, 0, 0, 0.548);
		z-index: 1000;
	}

	.modal-content {
		background: var(--background);
		border-radius: var(--border-radius);
		max-width: var(--width);
		width: 95%;
		position: absolute;
		left: 50%;
		top: 10%; // 5rem
		transform: translateX(-50%);
		padding: 1.5rem;
		overflow-y: var(--overflow);
		max-height: 85%;
		min-height: var(--min-height);
	}

	.close-icon {
		position: absolute;
		top: 0.5rem;
		right: 0.5rem;
		color: var(--text-light);
	}

</style>