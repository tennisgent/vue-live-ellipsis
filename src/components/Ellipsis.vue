<template>
	<div
		class="ellipsis-content"
		ref="content"
	/>
</template>

<script>
const DEFAULT_TAIL_CHAR = '&hellip;'
const CHARS_PER_CHECK = 2
const positions = {
	START: 'start',
	MIDDLE: 'middle',
	END: 'end',
}
const positionValues = Object.keys(positions).map((key) => positions[key])

const startEllipsis = (text, maxChars) => {
	if (!text) return ''
	return `${DEFAULT_TAIL_CHAR}${text.substr(-maxChars, maxChars).trim()}`
}

const middleEllipsis = (text, maxChars) => {
	if (!text) return ''
	const count = Math.floor(maxChars / 2)
	const first = text.substr(0, count).trim()
	const last = text.substr(-count, count).trim()
	return `${first}${DEFAULT_TAIL_CHAR}${last}`
}

const endEllipsis = (text, maxChars) => {
	if (!text) return ''
	return `${text.substr(0, maxChars).trim()}${DEFAULT_TAIL_CHAR}`
}

const getContentAtLength = (text, maxChars, position) => {
	switch (position) {
		case positions.START:
			return startEllipsis(text, maxChars)
		case positions.MIDDLE:
			return middleEllipsis(text, maxChars)
		default:
			return endEllipsis(text, maxChars)
	}
}

const isOverflowing = (container, child) => {
	const containerRect = container.getBoundingClientRect()
	const contentRect = child.getBoundingClientRect()
	return (
		containerRect.height < contentRect.height ||
		containerRect.width < contentRect.width
	)
}

export default {
	name: 'Ellipsis',
	props: {
		text: {
			type: String,
			default: '',
		},
		position: {
			type: String,
			default: positions.END,
			validator: (value) => positionValues.includes(value),
		},
	},
	data() {
		return { observers: null }
	},
	methods: {
		isOverflowing() {
			const { content } = this.$refs
			return isOverflowing(content.parentElement, content)
		},
		setContent(content) {
			this.$refs.content.innerHTML = content
		},
		setContentAtLength(text, length, position) {
			this.setContent(getContentAtLength(text, length, position))
		},
		shrinkUntilNotOverflowing(text, position) {
			for (let length = text.length; length > 0; length -= CHARS_PER_CHECK) {
				if (!this.isOverflowing()) {
					// We shrank far enough. Now time to stop.
					break
				}
				// Keep shrinking...
				this.setContentAtLength(text, length, position)
			}
		},
		recalc() {
			requestAnimationFrame(() => {
				this.setContent(this.text)
				this.shrinkUntilNotOverflowing(this.text, this.position)
			})
		},
	},
	watch: {
		text: function (val, old) {
			if (val && val !== old) {
				this.recalc()
			}
		},
		maxLength: function (val, old) {
			if (val && val !== old) {
				this.recalc()
			}
		},
		position: function (val, old) {
			if (val && val !== old) {
				this.recalc()
			}
		},
	},
	mounted() {
		const parent = this.$refs.content.parentElement
		const resizeObserver = new ResizeObserver(this.recalc)
		resizeObserver.observe(parent)
		const mutationObserver = new MutationObserver(this.recalc)
		mutationObserver.observe(parent, {
			childList: false,
			attributes: true,
		})
		this.observers = [resizeObserver, mutationObserver]
	},
	beforeDestroy() {
		this.observers.forEach((ob) => ob.disconnect())
		this.observers.length = 0
	},
}
</script>

<style scoped>
.ellipsis-container {
	max-width: 100%;
	overflow: hidden;
}
.ellipsis-container.multi-line {
	height: 100%;
}
.ellipsis-content {
	display: inline;
	white-space: nowrap;
}
.ellipsis-content.multi-line {
	white-space: normal;
}
</style>
