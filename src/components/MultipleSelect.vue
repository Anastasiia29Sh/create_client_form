<template>
  <div class="mult-select">
    <div class="select" @click.self="isOpenSelect = !isOpenSelect">
      <span
        class="select-placeholder"
        v-if="selected.length === 0"
        @click.self="isOpenSelect = !isOpenSelect"
      >
        -- Выберите группу --
      </span>
      <span
        class="client-select"
        v-for="(client, index) in selected"
        :key="index"
        :value="client"
        @click.stop="del(client)"
      >
        {{ client }}
      </span>
    </div>

    <div class="client-options" v-if="isOpenSelect">
      <span
        v-for="(client, index) in client_group"
        :key="index"
        :value="client"
        @click="choose(client)"
      >
        {{ client }}
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: "MultipleSelect",
  props: {
    client_group: Array,
  },
  data() {
    return {
      selected: [],
      isOpenSelect: false,
    };
  },

  methods: {
    choose(client) {
      if (!this.selected.includes(client)) this.selected.push(client);
      this.isOpenSelect = false;
      this.$emit("selectClientGroups", this.selected);
    },
    del(client) {
      this.selected = this.selected.filter((el) => el !== client);
      this.$emit("selectClientGroups", this.selected);
    },
  },
};
</script>

<style scoped lang="sass">
$firstColor: #00a4af
$secondColor: #aeaeae

.mult-select
	position: relative
	border: 1px solid $secondColor
	border-radius: 7px
	cursor: pointer

	.select
		display: flex
		flex-wrap: wrap
		gap: 5px
		padding: 7px
		border-radius: 7px
		&::after
			content: "\2039"
			font-size: 20px
			font-weight: bold
			margin-left: auto
			transform: rotate(270deg)

		.select-placeholder
			-ms-user-select: none
			-moz-user-select: none
			-webkit-user-select: none
			user-select: none

		.client-select
			background-color: $firstColor
			padding: 4px
			color: #ffffff
			border-radius: 7px
			&::after
				content: "\2716"
				color: #000000
				font-size: 12px
				padding-left: 5px

	.client-options
		position: absolute
		width: 100%
		padding-top: 10px
		border: 1px solid #000000
		background-color: #ffffff
		-webkit-box-shadow: 0px 5px 8px 0px rgba(136, 136, 136, 0.2)
		-moz-box-shadow: 0px 5px 8px 0px rgba(136, 136, 136, 0.2)
		box-shadow: 0px 5px 8px 0px rgba(136, 136, 136, 0.2)
		span
			display: block
			padding: 10px
			&:hover
				background-color: rgba($firstColor, 0.3)
</style>
