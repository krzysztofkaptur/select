<template>
	<div class="multiselect" v-click-outside-close="closeDropdown" :id="`multiselect-${Math.random()}`">
		<div class="multiselect__placeholder" @click="toggleDropdown">
			<span class="multiselect__label" :class="[placeholder.length && 'multiselect__label--active']">
				{{ filter.mainLabel.label }}
			</span>
			<span class="multiselect__selected" v-show="placeholder.length">
				{{ placeholder }}
			</span>
			<button class="multiselect__arrow">
				<div
					class="multiselect__arrow-icon"
					:class="{
						'multiselect__arrow-icon--rotate': isDropdownOpened,
					}"
				></div>
			</button>
		</div>
		<button class="multiselect__close" @click="clearChosen" v-show="chosenOptions.length && !isDropdownOpened">
			<div class="multiselect__close-icon">X</div>
		</button>
		<ul class="multiselect__options" v-show="isDropdownOpened">
			<VueCustomScrollbar class="scroll-area" :settings="scrollbarSettings">
				<li
					v-for="(option, index) in filter.options"
					:key="`${option.value}-${index}`"
					@click="chooseOption(option)"
					class="multiselect__option"
					:class="[chosenOptions.some((item) => item.value === option.value) && 'multiselect__option--active']"
				>
					<span :data-value="option.value">
						{{ option.label }}
					</span>
				</li>
			</VueCustomScrollbar>
		</ul>
	</div>
</template>

<script>
	import VueCustomScrollbar from "vue-custom-scrollbar";
	import "vue-custom-scrollbar/dist/vueScrollbar.css";

	export default {
		components: {
			VueCustomScrollbar,
		},
		props: {
			filter: {
				type: Object,
				default: () => {},
				required: true,
			},
			chosenOptions: {
				type: Array,
				default: () => [],
			},
			isSingle: {
				type: Boolean,
				default: false,
			},
		},
		directives: {
			"click-outside-close": {
				bind: function(el, binding, vNode) {
					// Provided expression must evaluate to a function.
					if (typeof binding.value !== "function") {
						const compName = vNode.context.name;
						let warn = `[Vue-click-outside-close:] provided expression '${binding.expression}' is not a function, but has to be`;
						if (compName) {
							warn += `Found in component '${compName}'`;
						}

						console.warn(warn);
					}

					// Define Handler and cache it on the element
					const bubble = binding.modifiers.bubble;
					const handler = (e) => {
						if (bubble || (!el.contains(e.target) && el !== e.target)) {
							binding.value(e);
						}
					};
					el.__vueClickOutside__ = handler;

					// add Event Listeners
					document.addEventListener("click", handler);
				},

				unbind: function(el, binding) {
					// Remove Event Listeners
					document.removeEventListener("click", el.__vueClickOutside__);
					el.__vueClickOutside__ = null;
				},
			},
		},
		data() {
			return {
				isDropdownOpened: false,
				scrollbarSettings: {
					suppressScrollY: false,
					suppressScrollX: false,
					wheelPropagation: false,
				},
			};
		},
		computed: {
			placeholder() {
				return this.isSingle ? this.chosenOptions.length && this.chosenOptions[0]?.label : this.chosenOptions.length && `Wybrano: ${this.chosenOptions.length}`;
			},
		},
		methods: {
			toggleDropdown() {
				this.isDropdownOpened = !this.isDropdownOpened;
			},
			closeDropdown() {
				this.isDropdownOpened = false;
			},
			chooseOption(option) {
				if (this.isSingle) {
					this.toggleDropdown();
				}

				this.$emit("select", {
					mainLabel: this.filter.mainLabel,
					option,
					isSingle: this.filter.isSingle,
				});
			},
			clearChosen() {
				this.$emit("clear", this.filter.mainLabel);
			},
			isActive(value) {
				return true;
			},
		},
	};
</script>

<style lang="scss">
	.multiselect {
		width: max-content;
		position: relative;

		&__label {
			position: relative;
			transform: translateY(1px);
			transition: transform 0.2s ease-in-out;
			user-select: none;

			&--active {
				transform: translateY(-20px);
			}
		}

		&__options {
			margin-top: 0;
			margin-left: 0;
			padding-left: 0;
			position: absolute;
			left: 0;
			right: 0;
			top: 100%;
			border: 1px solid #ddd;
			border-radius: 0 0 5px 5px;
			background-color: white;
			z-index: 100;
		}

		&__option {
			padding: 6px 10px;
			text-align: left;
			user-select: none;

			&--active {
				background-color: red;
				color: white;
			}
		}

		&__placeholder {
			display: flex;
			align-items: center;
			min-height: calc(1.5em + 0.75rem + 2px);
			position: relative;
			border: 1px solid #ddd;
			padding-left: 10px;
			padding-right: 40px;
			user-select: none;
		}

		&__arrow {
			border: none;
			position: absolute;
			background-color: transparent;
			right: 0;

			&-icon {
				width: 0;
				height: 0;
				border-left: 5px solid transparent;
				border-right: 5px solid transparent;
				border-top: 5px solid #f00;

				transition: transform 0.2s ease-in-out;
				&--rotate {
					transform: rotate(180deg);
				}
			}
		}

		&__close {
			border: none;
			position: absolute;
			background-color: transparent;
			right: 20px;
			z-index: 50;
			padding: 0;
			width: 20px;
			height: 20px;
			top: calc(50% - 10px);
			user-select: none;
		}

		&__selected {
			position: absolute;
			left: 10px;
			right: 40px;
			max-width: 100%;
			white-space: nowrap;
			overflow: hidden;
			text-overflow: ellipsis;
			text-align: left;
		}

		.scroll-area {
			max-height: 100px;
			overflow: hidden;
		}
	}
</style>
