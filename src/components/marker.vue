<template>
  <div></div>
</template>

<script>
export default {
  name: 'GMapMarker',
  props: {
    position: {
      type: Object,
      required: true,
    },
    map: {
      type: Object,
      required: true,
    },
    title: {
      type: String,
      default: '',
    },
    draggable: {
      type: Boolean,
      default: false,
    },
    zIndex: {
      type: Number,
      default: null,
    },
    content: {
      type: [String, Object],
      default: null, // per custom HTML nel marker
    },
  },
  data() {
    return {
      markerObject: null,
    };
  },
  watch: {
    position: {
      handler(val) {
        if (this.markerObject) {
          this.markerObject.position = val;
        }
      },
      deep: true,
    },
  },
  mounted() {
    this.addMarker();
  },
  unmounted() {
    this.removeMarker();
  },
  methods: {
    addMarker() {
      if (!google.maps.marker?.AdvancedMarkerElement) {
        console.error('AdvancedMarkerElement non disponibile. Hai caricato la libreria "marker"?');
        return;
      }

      const options = {
        map: this.map,
        position: this.position,
        title: this.title,
        zIndex: this.zIndex,
        draggable: this.draggable,
      };

      if (this.content) {
        if (typeof this.content === 'string') {
          const contentDiv = document.createElement('div');
          contentDiv.innerHTML = this.content;
          options.content = contentDiv;
        } else if (this.content instanceof HTMLElement) {
          options.content = this.content;
        }
      }

      this.markerObject = new google.maps.marker.AdvancedMarkerElement(options);

      this.$emit('marker-created', this.markerObject);
    },
    removeMarker() {
      if (this.markerObject) {
        this.markerObject.map = null;
        this.markerObject = null;
      }
    },
  },
};
</script>
