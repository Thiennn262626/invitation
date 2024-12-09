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
    <el-main class="main" @scroll="handleScroll">
      <section ref="section1" class="section invitation">
        <h2>Thư Mời</h2>
        <p>Nội dung thư mời...</p>
      </section>
      <section ref="section2" class="section guide">
        <h2>Hướng Dẫn</h2>
        <p>Nội dung hướng dẫn...</p>
      </section>
      <section ref="section3" class="section thanks">
        <h2>Cảm Ơn</h2>
        <p>Nội dung cảm ơn...</p>
      </section>
    </el-main>
  </div>
</template>

<script>
export default {
  data() {
    return {
      activeTab: '1', // Tab đang được chọn
    }
  },
  methods: {
    handleMenuClick(index) {
      // Scroll đến phần nội dung tương ứng
      const sectionRefs = {
        1: this.$refs.section1,
        2: this.$refs.section2,
        3: this.$refs.section3,
      }
      const target = sectionRefs[index]
      if (target) {
        target.scrollIntoView({ behavior: 'smooth' })
      }
    },
    handleScroll() {
      // Lấy vị trí hiện tại của các phần nội dung
      const sections = [
        { index: '1', element: this.$refs.section1 },
        { index: '2', element: this.$refs.section2 },
        { index: '3', element: this.$refs.section3 },
      ]

      // Xác định phần nội dung nào đang nằm trong vùng nhìn thấy
      const currentSection = sections.find((section) => {
        const rect = section.element.getBoundingClientRect()
        return rect.top <= 100 && rect.bottom > 100 // Phần nội dung đang hiển thị
      })

      // Cập nhật trạng thái tab nếu cần
      if (currentSection && currentSection.index !== this.activeTab) {
        this.activeTab = currentSection.index
      }
    },
  },
  mounted() {
    // Gắn sự kiện scroll vào phần tử chính
    const mainElement = this.$el.querySelector('.main')
    mainElement.addEventListener('scroll', this.handleScroll)
  },
  beforeUnmount() {
    // Hủy sự kiện scroll khi component bị gỡ bỏ
    const mainElement = this.$el.querySelector('.main')
    mainElement.removeEventListener('scroll', this.handleScroll)
  },
}
</script>

<style>
/* CSS giữ nguyên như ban đầu */
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
  justify-content: center;
}

.main {
  flex: 1;
  overflow-y: auto;
}

.section {
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  border-bottom: 1px solid #ddd;
  margin-bottom: 20px;
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
/* Toàn bộ thanh cuộn */
::-webkit-scrollbar {
  width: 8px; /* Độ rộng của thanh cuộn dọc */
  height: 8px; /* Độ cao của thanh cuộn ngang */
}

/* Phần track (nền của thanh cuộn) */
::-webkit-scrollbar-track {
  background: #f0f0f0; /* Màu nền */
  border-radius: 4px; /* Bo góc */
}

/* Phần thumb (thanh kéo) */
::-webkit-scrollbar-thumb {
  background: #409eff; /* Màu của thanh kéo */
  border-radius: 4px; /* Bo góc */
  border: 2px solid #f0f0f0; /* Khoảng cách giữa thumb và track */
}

/* Hover vào thanh kéo */
::-webkit-scrollbar-thumb:hover {
  background: #0073e6; /* Màu khi hover */
}
</style>
