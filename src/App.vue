<template>
  <div id="app">
    <!-- Header -->
    <el-header class="header">
      <el-menu
        mode="horizontal"
        :default-active="activeTab"
        @select="handleMenuClick"
        class="menu"
      >
        <el-menu-item index="1">Thư mời</el-menu-item>
        <el-menu-item index="2">Hướng dẫn</el-menu-item>
        <el-menu-item index="3">Cảm ơn</el-menu-item>
      </el-menu>
    </el-header>

    <!-- Body -->
    <el-main class="main">
      <h2 class="invitation-title">Thư mời gửi đến {{ name }} !!!</h2>
      <section ref="section1" class="section invitation">
        <el-image
          :src="images.section1"
          alt="Mô tả hình ảnh 1"
          fit="cover"
          class="full-image"
          @error="handleImageError"
        />
      </section>
      <section ref="section2" class="section guide">
        <el-image
          :src="images.section2"
          alt="Mô tả hình ảnh 2"
          fit="cover"
          class="full-image"
          @error="handleImageError"
        />
      </section>
      <section ref="section3" class="section thanks">
        <el-image
          :src="images.section3"
          alt="Mô tả hình ảnh 3"
          fit="cover"
          class="full-image"
          @error="handleImageError"
        />
      </section>
    </el-main>
  </div>
</template>
<script>
import DesktopImageC from "@/assets/c.png";
import MobileImageA from "@/assets/a1.png";
import MobileImageB from "@/assets/b1.png";
import MobileImageC from "@/assets/c1.png";

export default {
  data() {
    return {
      activeTab: "1",
      images: {
        section1: require("@/assets/a.png"),
        section2: require("@/assets/b.png"),
        section3: DesktopImageC,
      },
      name: "bạn",
    };
  },
  methods: {
    detectDevice() {
      const userAgent = navigator.userAgent;
      // Nếu là thiết bị di động, thay đổi đường dẫn ảnh
      if (/Mobi|Android|iPhone|iPad|iPod/i.test(userAgent)) {
        this.images = {
          section1: MobileImageA,
          section2: MobileImageB,
          section3: MobileImageC,
        };
      }
    },
    getQueryParameter(param) {
      const urlParams = new URLSearchParams(window.location.search);
      return urlParams.get(param);
    },
    handleMenuClick(index) {
      const sectionRefs = {
        1: this.$refs.section1,
        2: this.$refs.section2,
        3: this.$refs.section3,
      };
      const target = sectionRefs[index];
      if (target) {
        target.scrollIntoView({ behavior: "smooth", block: "start" });
      }
    },
    handleScroll() {
      const sections = [
        { index: "1", element: this.$refs.section1 },
        { index: "2", element: this.$refs.section2 },
        { index: "3", element: this.$refs.section3 },
      ];

      const currentSection = sections.find((section) => {
        const rect = section.element.getBoundingClientRect();
        const viewportHeight =
          window.innerHeight || document.documentElement.clientHeight;
        return rect.top >= 0 && rect.top <= viewportHeight / 2;
      });

      if (currentSection && currentSection.index !== this.activeTab) {
        this.activeTab = currentSection.index;
      }
    },
    handleImageError(event) {
      event.target.src = DesktopImageC;
    },
  },
  mounted() {
    this.detectDevice(); // Gọi khi ứng dụng được tải
    const mainElement = this.$el.querySelector(".main");
    mainElement.addEventListener("scroll", this.handleScroll);
    // Lấy tham số name từ URL
    const nameFromQuery = this.getQueryParameter("name");
    if (nameFromQuery) {
      this.name = decodeURIComponent(nameFromQuery); // Giải mã nếu cần
    }
  },
  beforeUnmount() {
    const mainElement = this.$el.querySelector(".main");
    mainElement.removeEventListener("scroll", this.handleScroll);
  },
};
</script>
<style>
/* CSS */
body {
  margin: 0;
  font-family: Arial, sans-serif;
}

#app {
  display: flex;
  flex-direction: column;
  height: 100vh;
}

.header {
  background-color: white;
  color: white;
  padding: 0;
}

.menu {
  line-height: 60px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.main {
  flex: 1;
  overflow-y: auto;
}
.invitation-title {
  text-align: center; /* Căn giữa theo chiều ngang */
  color: #9f7736; /* Màu chữ */
  font-size: 24px; /* Kích thước chữ */
  font-weight: bold; /* Đậm chữ */
  /* margin: 20px 0; Khoảng cách trên/dưới */
}
.section {
  height: 95vh;
  display: flex;
  position: relative; /* Đảm bảo layout */
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  border-bottom: 1px solid #ddd;
}

.el-image {
  height: 100%;
}

.invitation {
  background-color: #f0f9ff;
}

.guide {
  background-color: #e6f7ff;
}

.thanks {
  background-color: #f0f5ff;
}

/* Custom scrollbar */
::-webkit-scrollbar {
  width: 8px;
  height: 8px;
}

::-webkit-scrollbar-track {
  background: #f0f0f0;
  border-radius: 4px;
}

::-webkit-scrollbar-thumb {
  background: #409eff;
  border-radius: 4px;
  border: 2px solid #f0f0f0;
  transition: background 0.3s;
}

::-webkit-scrollbar-thumb:hover {
  background: #005bbb;
}
</style>
