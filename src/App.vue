<template>
  <div id="app">
    <div class="drawApp">
      <div class="leftPanel">
        <button @click="add">ADD</button>
      </div>
      <div ref="mainCanvas" class="mainCanvas" @click="clickOutside">
        <SpriteImage
          v-for="item in items"
          :key="item.id"
          :item="item"
          :selected="selectedId === item.id"
          :onUpdate="onUpdate"
          :mainCanvasRect="mainCanvasRect"
          @onselect="onSelect"
        />
      </div>
    </div>
  </div>
</template>

<script>
// import Sprite from "@/components/Sprite.vue";
import SpriteImage from "@/components/SpriteImage.vue";

const colors = ["red", "blue", "green"];
const image =
  "https://storage.googleapis.com/co_backham_me/images/vrmonkey.png";

export default {
  name: "App",
  components: {
    // Sprite,
    SpriteImage,
  },
  data: () => {
    return {
      items: [],
      selectedId: null,
      mainCanvasRect: { x: 100, y: 100, height: 100, width: 100, scale: 1.0 },
    };
  },
  computed: {
    getViewCenter() {
      // TODO 設定した描画領域のサイズを取得
      return { x: window.innerWidth / 2, y: window.innerHeight / 2 };
    },
  },
  mounted() {
    // キャンバスのサイズを取得
    // 座標・スケールの正規化をする必要がある
    const mainCanvas = this.$refs.mainCanvas;
    let mainCanvasRect = mainCanvas.getBoundingClientRect();
    mainCanvasRect.scale = 1.0;
    this.mainCanvasRect = {
      x: mainCanvasRect.x,
      y: mainCanvasRect.y,
      width: mainCanvasRect.width,
      height: mainCanvasRect.height,
      scale: mainCanvasRect.scale,
    };
    console.log({ ...this.mainCanvasRect });
  },
  methods: {
    add() {
      const index = Math.floor(Math.random() * colors.length);
      const items = [...this.items];
      const id = items.length + 1;
      items.push({
        id,
        color: colors[index],
        image,
        transform: {
          x: this.getViewCenter.x,
          y: this.getViewCenter.y,
          width: 100,
          height: 100,
          rotation: 0,
        },
      });
      this.items = items;
      this.selectedId = id;
    },
    onUpdate(id, transform) {
      console.log(id, { ...transform });
      let items = [...this.items];
      items = items.map((item) => {
        if (item.id === id) {
          item.transform = transform;
        }
        return item;
      });
      this.items = items;
      console.log([...this.items]);
    },
    onSelect(id) {
      console.log("select", id);
      this.selectedId = id;
    },
    clickOutside() {
      // this.selectedId = null;
    },
  },
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding: 0;
  margin: 0;
}

.drawApp {
  display: flex;
  height: 100vh;
  width: 100vw;
}
.leftPanel {
  width: 120px;
  border-right: 1px solid black;
  background-color: #efefef;
}
.mainCanvas {
  flex: 1;
  background-color: #eee;
  position: relative;
}
</style>
