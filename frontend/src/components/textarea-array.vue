<template>
    <q-input
    :label="label"
    stack-label
    v-model="dataString"
    type="textarea"
    @input="updateParent"
    outlined 
    :readonly="readonly"
    :needs_url_validate="needs_url_validate"
    />
</template>

<script>

export default {
    name: 'textarea-array',
    props: {
        label: String,
        value: Array,
        noEmptyLine: {
            type: Boolean,
            default: false
        },
        readonly: {
            type: Boolean,
            default: false
        },
        needs_url_validate: {
            type: Boolean,
            default: false
        }
    },

    data: function() {
        return {
            dataString: ""
        }
    },

    mounted: function() {
        if (this.value)
            this.dataString = this.value.join('\n')
    },

    watch: {
        value (val) {
            var str = (val)? val.join('\n'): ""
            if (str === this.dataString)
                return
            this.dataString = str
        }
    },

    methods: {
        updateParent: function() {
            if (this.noEmptyLine)
                this.$emit('input', this.dataString.split('\n').filter(e => e !== ''))
            else
                this.$emit('input', this.dataString.split('\n'))
        },

        url_val: function() {
            if (!this.needs_url_validate)
                return true
            let huy = this.dataString.split('\n')
            for (let i in huy){
                if (huy[i] !== '' && !huy[i].match(/^((https?|ftps?):\/\/)?([\wА-я-]+\.)*[\wА-я-]+\/?$/)){
                    return false
                    }
            }
            return true
        }
    },
}

</script>

<style>
</style>