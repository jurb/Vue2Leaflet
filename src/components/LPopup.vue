<script>
import propsBinder from '../utils/propsBinder.js';
import findRealParent from '../utils/findRealParent.js';
import { optionsMerger } from '../utils/optionsUtils.js';
import Popper from '../mixins/Popper.js';
import Options from '../mixins/Options.js';

export default {
  name: 'LPopup',
  mixins: [Popper, Options],
  props: {
    latLng: {
      type: [Object, Array],
      default: () => []
    }
  },
  mounted () {
    const options = optionsMerger(this.popperOptions, this);
    this.mapObject = L.popup(options);
    if (this.latLng !== undefined) {
      this.mapObject.setLatLng(this.latLng);
    }
    L.DomEvent.on(this.mapObject, this.$listeners);
    propsBinder(this, this.mapObject, this.$options.props);
    this.mapObject.setContent(this.content || this.$el);
    this.parentContainer = findRealParent(this.$parent);
    this.parentContainer.mapObject.bindPopup(this.mapObject);
  },
  beforeDestroy () {
    if (this.parentContainer.mapObject && this.parentContainer.mapObject.getPopup()) {
      this.parentContainer.mapObject.unbindPopup();
    }
  }
};
</script>
