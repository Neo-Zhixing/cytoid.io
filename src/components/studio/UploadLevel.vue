<template lang="pug">
upload.upload-level(
  accept=".cytoidlevel,.zip"
  type="levels/packages"
  callback
  @upload="upload"
)
  template(v-slot:title)
    slot(name="text")
      | Click or drag a Cytoid level to this area
  template(v-slot:subtitle)
    slot(name="hint")
      | Don't know how to create one? Read our&nbsp;
      a(href="https://github.com/Cytoid/Cytoid/wiki/a.-Creating-a-level" @click.stop) wiki
      | !
</template>

<script>
import gql from 'graphql-tag'
import Upload from '../Upload'
export default {
  components: {
    Upload
  },
  props: {
    level: {
      type: Object,
      required: false,
      default: null,
    }
  },
  methods: {
    upload (data) {
      this.$apollo.mutate({
        mutation: gql`mutation UnpackLevel($token: String!, $replace: String) {
          package: unpackLevelPackage(token: $token, replace: $replace) {
            id
            uid
            title
          }
        }`,
        variables: {
          token: data.token,
          replace: this.level?.uid,
        }
      })
        .then((res) => {
          const uid = res.data?.package?.uid
          const title = res.data?.package?.title
          this.$buefy.toast.open({
            message: `${title} (${uid}) Uploaded`,
            type: 'is-success'
          })
          if (uid) {
            this.$router.push({ name: 'levels-id-manage', params: { id: uid } })
          }
        })
        .catch((error) => {
          this.$buefy.toast.open({
            message: error.message,
            type: 'is-danger'
          })
        })
        .then(data.callback)
    }
  }
}
</script>

<style lang="scss">
.upload-level .upload-draggable {
  transition: 0.2s $hoverEasing;
  border: none !important;
  background-color: #FF3CAC;
  background-image: linear-gradient(225deg, #FF3CAC 0%, #784BA0 50%, #2B86C5 100%) !important;
  box-shadow: 0 0 0 0 hsla(226, 68%, 67%, 0.5);
  &:hover {
    box-shadow: 0 0 0 2px hsla(226, 68%, 67%, 0.5);
  }
}
</style>
